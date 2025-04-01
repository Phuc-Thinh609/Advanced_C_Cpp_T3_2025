# ADVANCE C
<details>
<summary>Bài 1: COMPILER - PREPROCESS - MACRO</summary>
 
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
</details>

<details>
<summary>Bài 2: STDARG - ASSERT</summary>

 # STDARG 
 - Thư viện __stdarg__ xử lý hàm có số tham số không cố định
 - Công dụng: cho phép viết các hàm nhận số lượng đối số khác nhau tại mỗi lần gọi, các hàm như __printf__, __scanf__
 - Các thành phần chính:

   | Thành phần | Công dụng |
   |------------|-----------|
   | __va_list__ | Khai báo danh sách các đối số biến đổi |
   | __va_start__ | Bắt đầu truy cập danh sách |
   | __va_arg__ | Lấy từng đối số trong danh sách |
   | __va_end__ | Kết thúc truy cập danh sách |
- Ví dụ - in giá trị
  ```Cpp
  void display(int count, ...) {
    va_list args;
    va_start(args, count);
    for (int i = 0; i < count; i++) {
        printf("Value at %d: %d\n", i, va_arg(args,int)); 
    }
    va_end(args);
  }
  ```
# ASSERT 
 - Thư viện __assert__ kiểm tra điều kiện khi chạy (Debug)
 - Công dụng:
   * Kiểm tra điều kiện logic trong chương trình
   * Nếu điều kiện sai -> in lỗi và dừng chương trình
   * Chỉ dừng khi debug, có thể tắt bằng __#define NDEBUG__
 - Ví dụ điển hình
   ```Cpp
     int x = 5;
     assert (x == 5);
     //Chương trình sẽ tiếp tục thực thi nếu điều kiện là đúng
     printf ("X is: %d", X)
   ```
- Ứng dụng thư viện assert
  | Macro tự định nghĩa | Chức năng |
  |---------------------|-----------|
  | __assert_in_range__(val, min, max) | Kiểm tra giá trị trong khoảng |
  | __assert_size(type, size)__ | Kiểm tra kích thước kiểu dữ liệu |
  - Ví dụ kiểm tra giá trị trong khoảng
    ```Cpp
      #define ASSERT_IN_RANGE(val, min, max) assert((val) >= (min) && (val) <= (max))
    
      void setLevel(int level) {
          ASSERT_IN_RANGE(level, 1, 10);
          // Thiết lập cấp độ
      }
    ```
  - Ví dụ kiểm tra kích thước kiểu dữ liệu
    ```Cpp
      #define ASSERT_SIZE(type, size) assert(sizeof(type) == (size))
    
      void checkTypeSizes() {
          ASSERT_SIZE(uint32_t, 4);
          ASSERT_SIZE(uint16_t, 2);
          // Kiểm tra các kích thước kiểu dữ liệu khác
      }
    ```
</details>

<details>
<summary>Bài 3: BITMAST</summary>

# Bitmask 
  * Bitmask là kỹ thuật thao tác với các bit riêng lẻ trong một biến
  * Mục đích của Bitmask là tối ưu hóa bộ nhớ
  * Sử dụng các phép toán bitwise (AND, OR, XOR, NOT, <<, >>) để bật, tắt, kiểm tra từng bit.
  ## NOT bitwise
  -  NOT bitwise hình dung là phép nghịch đảo (đảo bit)
      ```cpp
        int a = 0b0101; //5
        int b = 0b1001; 
        int result_1 = ~a; // 1010
        int result_2 = ~b; // 0110
      ```
  ## AND bitwise
  - AND bitwise hình dung là phép nhân
    ```Cpp
      int a = 0b0101; //5
      int b = 0b1001; 
      int result = a & b; // 0001
    ```
  ## OR bitwise
  - OR bitwise hình dung là phép cộng
    ```Cpp
    int a = 0b0101; //5
    int b = 0b1001; 
    int result = a | b; // 1101
    ```
  ## XOR bitwise
  - XOR bitwise hình dung dễ hiểu là đếm tổng số 1 trong cùng một cột, nếu lẻ là 1, ngược lại chẵn là 0
    ```Cpp
    int a = 0b0101; //5
    int b = 0b1001; 
    int c = 0b1101; 
    int d = 0b1011; 
    int result = a ^ b ^ c ^ d; // 0b1010
    ```
 ## Shifl left và Shifl right bitwise 
 - Shifl left (<<) và Shifl right (>>) hình dung là dịch bên nào thì bên đó xóa và thêm số 0 và bên ngược lại
   ```Cpp
    int a = 0b10010110
    int result_1 = a << 1; //0b 00101100
    int result_2 = a << 3; //0b 10110000
    
    int result_3 = a >> 3; //0b 00010010
    int result_4 = a >> 6; //0b 00000010
   ```
</details>

<details>
<summary>Bài 5: STORAGE CLASSES</summary>

</details>

</details>

<details>
<summary>Bài 4: POINTER</summary>

</details>

</details>

<details>
<summary>Bài 6: GOTO - SETJMP.h</summary>

</details>

</details>

<details>
<summary>Bài 7: STRUCT - UNION</summary>

</details>
