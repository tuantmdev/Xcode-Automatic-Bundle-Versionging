# Xcode Automatic Bundle Versioning

## Mục đích

Tự động quản lý build version mỗi khi submit lên Fabric hoặc Appstore. Build version được quy định theo một mẫu định sẵn, cụ thể sẽ là dạng reversed datetime (YYYYMMDD.hhmmss).

## Cách thực hiện

1. Mở menu scheme ở góc trên, bên trái của Xcode (phía bên trái của phần chọn device) và chọn Edit Scheme…

    ![Imgur](http://i.imgur.com/Nc9umM5.png)

2. Tìm “Archive” (hoặc action bất kì mà bạn muốn build version thay đổi khi thực hiện) trong danh sách action ở cột bên trái, expand action đó và chọn vào mục “Pre-actions”
    
    ![Imgur](http://i.imgur.com/TVch5DA.png)

3. Nếu không có script nào ở đây thì bấm vào biểu tượng dấu cộng ở góc dưới bên trái cửa sổ Edit Scheme và chọn “New Run Script Action”
    
    ![Imgur](http://i.imgur.com/cibf3nM.png)    

4. Mục Provide build settings from: <Chọn target mà bạn muốn build>
    
    ![Imgur](http://i.imgur.com/x1QQEUv.png)

5. Sau đó, paste đoạn code dưới đây vào phần điền script (Bạn có thể thay đổi định dạng nếu muốn):

    ```
    formatDate=$(date "+%Y%m%d.%H%M%S")
buildNumber="${formatDate}"
/usr/libexec/PlistBuddy -c "Set :CFBundleVersion ${buildNumber}" "${PROJECT_DIR}/${INFOPLIST_FILE}"
```

    ![Imgur](http://i.imgur.com/vS3b0TN.png)

Trong đoạn script trên, bạn có thể thấy build number được tạo bằng một string từ giá trị thời gian hiện tại. Chuỗi này đảm bảo sẽ duy nhất nếu chúng ta không `Archive` nhiều hơn 1 lần trong 1 giây (Và tôi chắc cũng không xảy ra trường hợp đó đâu). Việc dùng giá trị thời gian cho build number sẽ giúp bạn quản lý phiên bản tốt hơn, ít nhất sẽ biết được thời gian chúng ta export ra app đó, từ đó có thể debug tốt hơn.

That's it. Thanks for reading!