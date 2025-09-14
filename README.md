# STM32F103 - LED PC13 Blink with Timer2 Interrupt

## Giới thiệu
Đoạn code này được viết cho vi điều khiển **STM32F103C8T6** (board BluePill), sử dụng **Standard Peripheral Library (SPL)**.  
Chương trình minh họa cách sử dụng **TIM2** để tạo ngắt mỗi 1ms, từ đó hiện thực:  
- Hàm `delay_ms()` dùng để tạo trễ.  
- Đếm thời gian trong ngắt để điều khiển LED **PC13** nhấp nháy.  

---

## Các phần chính của code

### 1. `main.c`
- Gọi các hàm cấu hình:
  - `GPIO_Config()` để thiết lập LED PC13.
  - `TIM2_Config()` để khởi tạo Timer2 và ngắt.
- Vòng lặp `while(1)` để sẵn (chưa thêm logic khác), LED nháy nhờ xử lý trong **ngắt TIM2**.

---

### 2. Cấu hình GPIO - `GPIO_Config()`
- Bật clock cho **GPIOC**.  
- Cấu hình chân **PC13** là **Output Push-Pull** tốc độ 2MHz.  
- Mặc định set mức cao → LED trên BluePill tắt (vì LED này **active-low**).

---

### 3. Cấu hình Timer2 - `TIM2_Config()`
- Bật clock cho **TIM2**.  
- Thiết lập:
  - Prescaler = 7200 - 1 → Tần số timer = 10 kHz (chu kỳ 0.1ms).  
  - Period = 10 - 1 → Ngắt xảy ra mỗi 1ms.  
- Bật ngắt TIM2 Update và cấu hình NVIC cho **TIM2_IRQn**.  
- Kết quả: Timer2 tạo ngắt **1ms một lần**.

---

### 4. Hàm Delay - `delay_ms(uint32_t ms)`
- Dùng biến `timer_ms` giảm dần trong ngắt TIM2.  
- Vòng lặp `while` chờ cho đến khi `timer_ms = 0`.  
- Hiện thực **delay bận (blocking delay)**.

---

### 5. Trình phục vụ ngắt - `TIM2_IRQHandler()`
- Xóa cờ ngắt TIM2.  
- Giảm giá trị `timer_ms` để phục vụ `delay_ms()`.  
- Sử dụng bộ đếm phụ `counter1000ms`:
  - Đếm đủ 1000 lần ngắt (tương đương 1000ms = 1 giây).  
  - Khi đủ, reset bộ đếm và đảo trạng thái LED **PC13**.  

---

## Kết quả
- LED **PC13** (trên board BluePill) nhấp nháy với chu kỳ **1000ms** (1 Hz, tức 1 lần sáng–tắt mỗi giây).  
- Hàm `delay_ms()` vẫn có thể dùng trong `main()` để tạo trễ chính xác bằng Timer2.  

---

## Mở rộng
- Thay đổi giá trị `counter1000ms` trong `TIM2_IRQHandler` để tạo tần số nháy LED khác (ví dụ: 500ms, 200ms, …).  
- Thêm nhiều chân GPIO khác và điều khiển LED hoặc thiết bị ngoại vi theo ý muốn.  
