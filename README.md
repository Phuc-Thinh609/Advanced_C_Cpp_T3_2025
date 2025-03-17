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

  1. *#include - chèn file tiêu đề*
  
  Dùng để nhập nội dung file *.h* vào chương trình, giúp tái sử dụng mã nguồn

  Ví dụ
```cpp
#include <stdio.h>  // Chèn thư viện chuẩn
```
  2. *#define - Định nghĩa hằng số và macro*
  3. *#undef - Hủy định nghĩa macro*
  4. *#if, #elif, #else - kiểm tra điều kiện tiền xử lý*
  5. *#ifdef, #ifndef - kiểm tra macro đã được định nghĩa chưa*
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

