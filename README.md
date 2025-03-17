# Compiler Process (Trình biên dịch)
 ## Giới thiệu về Compiler 
Compiler (Trình biên dịch) là công cụ dịch mã nguồn thành mã máy để chương trình có thể chạy. 

 ## Compiler Process
Quá trình biên dịch bao gồm: 
* Preprocessor (Tiền xử lý) -> tạo file *.*
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
     * Định nghĩa hằng số (slide 9)
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
     * Định nghĩa macro tính toán (slide 10)
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
     * Chỉ thị *#undef* dùng để hủy định nghĩa của một macro đã được định nghĩa trước đó bằng *#define*
       ```cpp
       #define SENSOR_DATA 42
       #undef SENSOR_DATA  // Hủy bỏ định nghĩa
       #define SENSOR_DATA 50  // Định nghĩa lại
         
  4. *#if, #elif, #else* - kiểm tra điều kiện tiền xử lý (slie 16)
     * *#if* đúng sẽ được biên dịch trong ngoặc, sai thì bỏ qua chạy đến gặp *#elif*
     * *#elif* dùng để thêm một điều kiện mới khi điều kiện trước đó if hoặc elif saisai
     * *#else* dùng khi không có điều kiện nào ở trên đúng
      ```cpp
       #include <stdio.h>
      
       typedef enum
       {
           GPIOA,
           GPIOB,
           GPIOC
       } Ports;
       
       typedef enum
       {
           PIN1,
           PIN2,
           PIN3,
           PIN4,
           PIN5,
           PIN6,
           PIN7,
       } Pins;
       
       typedef enum
       {
           HIGH,
           LOW
       } Status;
       
       #define STM32 0
       #define ATMEGA 1
       #define PIC 2      
       #define MCU STM32
       
       #if MCU == STM32
       void daoTrangThaiDen(Ports port, Pins pin, Status status)
       {
           if (status == HIGH)
           {
               HAL_GPIO_WritePin(port, pin, LOW);
           }
           else
           {
               HAL_GPIO_WritePin(port, pin, HIGH);
           }  
       }
       #elif MCU == ATMEGA
       void daoTrangThaiDen(Pins pin, Status status)
       {
           if (status == HIGH)
           {
               digitalWrite(pin, LOW);
           }
           else
           {
               digitalWrite(pin, HIGH);
           }  
       }
       
       #endif
       
       void delay(int ms)
       {
       
       }
            
       int main()
       {
           while(1)
           {
               daoTrangThaiDen(GPIOA,13,HIGH);
               delay(1000);
           }     
           return 0;
       }

  5. *#ifdef, #ifndef* - kiểm tra macro đã được định nghĩa chưa (slide 17)
     * *#ifdef* dùng để kiểm tra một macro đã được định nghĩa hay chưa, nếu macro đã được định nghĩa thì mã nguồn sau *#ifdef* sữ được biên dịch
     * *#ifndef* dùng để kiểm tra một macro đã được định nghĩa hay chưa, nếu macro chưa được định nghĩa thì mã nguồn sau *#ifndef* sữ được biên dịch
       ```cpp
       #ifndef __ABC_H
       #define __ABC_H
       int a = 10;
       #endif

  6. Một số toán tử trong macro

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

