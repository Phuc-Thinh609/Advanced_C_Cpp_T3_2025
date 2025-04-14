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
 - Storage Classes là biến đặc biệt, những từ khóa đi kèm với biến
 
 # Extern
  * Extern tận dụng lại những cái biến, những hàm của file khác (đã được định nghĩa) cho phép chương trình truy cập nó mà không cần định nghĩa lại
  * file.c (định nghĩa) -> file.h (extern) -> main.c 
  * Ứng dụng: chia sẻ tài nguyên giữa các module/file khác nhau
  * Note: extern chỉ sử dụng cho biến toàn cục. Vì biến cục bộ bị giới hạn trong hàm
 # Static
  ## Static local
   * Khi static được sử dụng với biến cục bộ (khai báo biến trong một hàm):
      + Giữ phạm vi của biến chỉ trong hàm đó
      + Giữ giá trị của biến qua các lần gọi hàm
   * Ví dụ:
     ```Cpp
        #include <stdio.h>
        
        void exampleFunction()
        {
            static int count = 0;  // Biến static giữ giá trị qua các lần gọi hàm
            count++;
            printf("Count: %d\n", count);
        }
        
        int main()
        {
            exampleFunction();  // In ra "Count: 1"
            exampleFunction();  // In ra "Count: 2"
            exampleFunction();  // In ra "Count: 3"
            return 0;
        }

     ```
  ## Static trong class
   * Khi một thành viên của **class** được khai báo là **static**, nó thuộc về class chứ không thuộc về các đối tượng cụ thể của class đó.
   * Các đối tượng của class sẽ chia sẻ cùng một bản sao của thành viên static, và nó có thể được truy cập mà không cần tạo đối tượng. Nó thường được sử dụng để lưu trữ dữ liệu chung của tất cả các đối tượng.
  ## Static global
   * Khi static được sử dụng với biến, hàm toàn cục, nó hạn chế phạm vi của biến, hàm đó chỉ trong file nguồn hiện tại
   * Ứng dụng: dùng để thiết kế các thư viện, quản lý tài nguyên nội bộ.
   * Lưu trữ trạng thái chung trong một file:
     + Dùng để lưu trữ dữ liệu mà các hàm trong cùng một file cần chia sẻ, nhưng không muốn các file khác can thiệp (đảm bảo tính đóng gói) 
      
 # Volatile 
  * Volatile một công cụ để đảm bảo rằng trình biên dịch xử lý chính xác các biến có thể thay đổi ngoài sự kiểm soát của mã chương trình. Ngắn chặn trình biên dịch tối ưu hóa hoặc xóa bỏ các thao tác trên biến đó, giữ cho các thao tác trên biến được thực hiện như đã được định nghĩa
  * Cú pháp:
    ```Cpp
    volatile type variable_name;
    ```
 * Ví dụ:
   ```Cpp

   #include <stdio.h>

   volatile int sensor_value; // Giả sử cảm biến cập nhật giá trị này
   
   int main() {
       while (sensor_value == 0) {
           // Chờ cho đến khi cảm biến thay đổi giá trị
       }
       printf("Sensor value changed to %d\n", sensor_value);
       return 0;
   }
   ```
  # Register
   * Register gợi ý trình biên dịch save biến thường dùng vào thanh ghi để truy cập nhanh hơn, thay vì RAM. Không được sử dụng cho biến toàn cục.
   * Ứng dụng: Tăng tốc độ xử lý
   * ![Sơ đồ minh họa](Register.png)
   * Ví dụ:
     ```Cpp
       #include <stdio.h>
       #include <time.h>
       
       int main()
       {
          // Lưu thời điểm bắt đầu
          clock_t start_time = clock();
          register int i;
       
          // Đoạn mã của chương trình
          for (i = 0; i < 2000000; ++i){}
       
          // Lưu thời điểm kết thúc
          clock_t end_time = clock();
       
          // Tính thời gian chạy bằng miligiây
          double time_taken = ((double)(end_time - start_time)) / CLOCKS_PER_SEC;
       
          printf("Thoi gian chay cua chuong trinh: %f giay\n", time_taken);
          return 0;
       }

     ```
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

# Struct
 * Trong C, Struct giúp mình định nghĩa một kiểu dữ liệu mới, dữ liệu mình mong muốn bao gồm các kiểu dữ kiệu khác. Struct cho phép tạo ra một thực thể dữ liệu lớn hơn và có tổ chức hơn từ các thành viên (members) của nó.
 * Kích thước của một struct bằng tổng kích thước của các members cộng thêm padding, để tối ưu bộ nhớ sắp xếp các members trong struct theo thứ tự kích thước giảm dần để giảm padding.
 * Cú pháp:
   ```Cpp
      struct TenStruct {
           kieuDuLieu1 thanhVien1;
           kieuDuLieu2 thanhVien2;
           //...
      };
   ```
 * Ứng dụng của struct trong:
   - Cấu hình (GPIO, UART,SPI, v.v)
   - JSON
   - Stack
   - Queue
   - Linked List 
# Union
 * Union là một cấu trúc dữ liệu, union cũng định nghĩa lại kiểu dữ liệu nhưng mục đích chính của union là tiết kiệm bộ nhớ bằng cách **chia sẻ cùng một vùng nhớ** cho các thành viên của nó.
 * Kích thước của một union bằng kích thước của thành viên lớn nhất, vì các thành viên chia sẻ cùng một vùng bộ nhớ.
 * Cú pháp:
   ```Cpp
      union TenUnion {
           kieuDuLieu1 thanhVien1;
           kieuDuLieu2 thanhVien2;
           //...
      };
   ```
# Ứng dụng kết hợp struct và union 

</details>
