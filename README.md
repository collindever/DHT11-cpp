# DHT11 cpp library for ESP32 (ESP-IDF)

This is an ESP32 cpp (esp-idf) library for the DHT11 low cost temperature/humidity sensors.

```
$ git clone https://github.com/collindever/DHT22-cpp.git

$ make menuconfig and make sure to set Component config->LWIP->recv_bufsize
$ make
$ make flash monitor
```

**USE**

See main.cpp

```C
	void DHT_task(void *pvParameter)
	{

		DHT dht;
		dht.setDHTgpio( (gpio_num_t)4 );
		printf( "Starting DHT Task\n\n");

		while(1) {

			printf("=== Reading DHT ===\n" );
			int ret = dht.readDHT();

			dht.errorHandler(ret);

			printf( "Hum %.1f\n", dht.getHumidity() );
			printf( "Tmp %.1f\n", dht.getTemperature() );

			// -- wait at least 2 sec before reading again ------------
			// The interval of whole process must be beyond 2 seconds !!
			vTaskDelay( 3000 / portTICK_RATE_MS );
		}
	}

	void app_main()
	{
		nvs_flash_init();
		vTaskDelay( 1000 / portTICK_RATE_MS );
		xTaskCreate( &DHT_task, "DHT_task", 2048, NULL, 5, NULL );
	}
```


