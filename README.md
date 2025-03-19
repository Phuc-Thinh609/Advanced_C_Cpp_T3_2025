# Compiler Process (Trình biên dịch)
 ## Giới thiệu về Compiler 
Compiler (Trình biên dịch) là công cụ dịch mã nguồn thành mã máy để chương trình có thể chạy. 

 ## Compiler Process
Quá trình biên dịch bao gồm: 
* Preprocessor (Tiền xử lý) -> tạo file *.i*
* Compiler (Biên dịch) -> tạo file *.s* (assembly)
* Assembler (Trình hợp dịch) -> tạo file *.o* (object)
* Linker (Trình liên kết) -> tạo file thực thi *.exe*

  Ví dụ:
  Mã nguồn (*.c, .h*) -> Preprocessor -> Biên dịch -> Hợp dịch -> Liên kết -> Tạo file chạy (*.exe*)

# Macro
 ## Giới thiệu về macro
- Macro là một khái niệm dùng để định nghĩa một tập hợp các hướng dẫn tiền xử lý
- Bản chất của Macro nó chỉ thay thế đoạn được định nghĩa *define* vào, giúp giảm lặp lại, dễ bảo trì chương trình
- Không phải code
- Macro không có kiểu dữ liệu
- Macro là các chỉ thị xử lý trước khi biên dịch. Các loại macro chính:

1. *#include* - chèn file tiêu đề
   
  Dùng để nhập nội dung file *.h* vào chương trình, giúp tái sử dụng mã nguồn

  Ví dụ
```cpp
#include <stdio.h>  // Chèn thư viện chuẩn
```
  2. *#define* - Định nghĩa hằng số và macro
     - Định nghĩa hằng số (slide 9)
       ```cpp
       #include <stdio.h>
       // Định nghĩa hằng số Pi sử dụng #define
       #define PI 3.14
       int main() {
           // Sử dụng hằng số Pi trong chương trình
           double radius = 5.0;
           double area = PI * radius * radius;
           printf("Radius: %.2f\n", radius);
           printf("Area of the circle: %.2f\n", area);
           return 0;
        }
     - Định nghĩa macro tính toán (slide 10)
       ```cpp
       #include <stdio.h>
       // Macro để tính bình phương của một số
       #define SQUARE(x) ((x) * (x))    
       int main() {
           // Sử dụng macro để tính bình phương của num
           int result = SQUARE(5);
           printf("Result is: %d\n", result);
           return 0;
       }

  3. *#undef* - Hủy định nghĩa macro
     - Chỉ thị *#undef* dùng để hủy định nghĩa của một macro đã được định nghĩa trước đó bằng *#define*
       ```cpp
       #define SENSOR_DATA 42
       #undef SENSOR_DATA  // Hủy bỏ định nghĩa
       #define SENSOR_DATA 50  // Định nghĩa lại
         
  4. *#if, #elif, #else* - kiểm tra điều kiện tiền xử lý (slie 16)
     - *#if* đúng sẽ được biên dịch trong ngoặc, sai thì bỏ qua chạy đến gặp *#elif*
     - *#elif* dùng để thêm một điều kiện mới khi điều kiện trước đó if hoặc elif saisai
     - *#else* dùng khi không có điều kiện nào ở trên đúng
      ```cpp
       #include <stdio.h>

       #define STM32 1
       #define AVR 2    
       #define MCU STM32  // Chọn loại vi điều khiển
       
       int main() {
           #if MCU == STM32
               printf("Bật đèn bằng HAL_GPIO_WritePin\n");
           #elif MCU == AVR
               printf("Bật đèn bằng digitalWrite\n");
           #else
               printf("Không hỗ trợ MCU này\n");
           #endif
       
           return 0;
       }

  5. *#ifdef, #ifndef* - kiểm tra macro đã được định nghĩa chưa (slide 17)
     - *#ifdef* dùng để kiểm tra một macro đã được định nghĩa hay chưa, nếu macro đã được định nghĩa thì mã nguồn sau *#ifdef* sữ được biên dịch
     - *#ifndef* dùng để kiểm tra một macro đã được định nghĩa hay chưa, nếu macro chưa được định nghĩa thì mã nguồn sau *#ifndef* sữ được biên dịch
       ```cpp
       #ifndef __ABC_H
       #define __ABC_H
       int a = 10;
       #endif

  6. Một số toán tử trong macro
     - Toán tử # chuyển tham số thành chuỗi
       ```cpp
       #define STRINGIZE(x) #x
       printf("%s", STRINGIZE(Hello)); // In ra "Hello"
      - Toán tử ## nối chuỗi
        ```cpp
        #define DECLARE_VARIABLE(prefix, number) int prefix##number;
        DECLARE_VARIABLE(var, 1); // Tạo biến var1
## Ví dụ minh họa macro dễ nhớ
 1. Tính diện tích hình tròn
    ```cpp
    #define PI 3.14
    double area = PI * radius * radius;
    ```

 2. Tìm max
    ```cpp
    #define MAX(x, y) ((x) > (y) ? (x) : (y))
    int maxNumber = MAX(10, 20); // Kết quả: 20
    ```
    
 3. Macro in menu (slide 25)
    ```cpp
    PRINT_MENU("Option 1", "Option 2", "Option 3", "Exit");
    ```
4. variadic
   - ... : biểu thị các đối số không xác định
   - __VA_ARGS__ : thay thế bằng danh sách các đối số
   ```Cpp
    #define print(...)__VA_ARGS__
   ```

