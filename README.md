Producer Task（產生隨機數）
 void mainTask(void *params){ 
	BlinkAgent blink(BLUE_LED_PAD);
 	blink.start("Blink", TASK_PRIORITY); 
printf("Main (Producer) Task started\n"); 
while (true) {
	 // 產生 8-bit 隨機數 (0 ~ 255) 
	uint8_t r = rand() & 0xFF; 
	// 傳送到 Queue 
	if (xQueueSend(xAssignmentQueue, &r, 10) != pdPASS) { 
	printf("Queue is full!\n");
	 } else { 
	printf("Sent to Queue: 0x%02X\n", r);
	 } 
	vTaskDelay(pdMS_TO_TICKS(3000)); // 每 3 秒送一次
                } 
}
Consumer Task（LED 顯示） 
void displayTask(void *params){ 
	CounterAgent counter( 
	LED0_PAD, LED1_PAD, LED2_PAD, 	LED3_PAD, LED4_PAD, LED5_PAD, 	LED6_PAD, LED7_PAD
	 ); 
uint8_t receivedVal;
 printf("Display Task started\n"); counter.start("Counter", TASK_PRIORITY); 
while (true) {
	 // 等待 Queue 資料（無限等待） 
	if (xQueueReceive(xAssignmentQueue, &receivedVal, portMAX_DELAY) == pdPASS) { 
	// 將 8-bit 值顯示到 LED counter.blink(receivedVal); 	printf("LED Display updated: 0x%02X\n", receivedVal); } } }

