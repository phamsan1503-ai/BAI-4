# BÀI TẬP THỐNG NHÚNG - Bài 4: Hẹn Giờ Bằng TIMER

Dự án minh họa sử dụng **Timer2** để tạo ngắt 500ms và điều khiển LED LD2 (PC13).

## Các phần chính
- **GPIO_Config**: Cấu hình PC13 làm output push-pull, ban đầu LED tắt.  
- **TIM2_Config**: Thiết lập TIM2 với prescaler và period để tạo ngắt mỗi 500ms, cấu hình NVIC.  
- **TIM2_IRQHandler**: Xử lý ngắt TIM2, đảo trạng thái LED trên PC13.  
- **main**: Gọi cấu hình GPIO và TIM2, LED tự nháy nhờ ngắt.  

## Kết quả
LED trên PC13 nháy chu kỳ **1 giây** (sáng 500ms, tắt 500ms).
