# HB: ultralcd_impl_DOGM.h


## Instructions
1. Remove or comment out `void lcd_bootscreen()` to remove default bootscreen.


## Code
```
void lcd_bootscreen() {
	#if ENABLED(SHOW_CUSTOM_BOOTSCREEN)
	  lcd_custom_bootscreen();
	#endif

	#if ENABLED(START_BMPHIGH)
	  constexpr uint8_t offy = 0;
	#else
	  constexpr uint8_t offy = DOG_CHAR_HEIGHT;
	#endif

	/*
	const uint8_t offx = (u8g.getWidth() - (START_BMPWIDTH)) / 2,
				  txt1X = (u8g.getWidth() - (sizeof(STRING_SPLASH_LINE1) - 1) * (DOG_CHAR_WIDTH)) / 2;

	u8g.firstPage();
	do {
	  u8g.drawBitmapP(offx, offy, (START_BMPWIDTH + 7) / 8, START_BMPHEIGHT, start_bmp);
	  lcd_setFont(FONT_MENU);
	  #ifndef STRING_SPLASH_LINE2
		u8g.drawStr(txt1X, u8g.getHeight() - (DOG_CHAR_HEIGHT), STRING_SPLASH_LINE1);
	  #else
		const uint8_t txt2X = (u8g.getWidth() - (sizeof(STRING_SPLASH_LINE2) - 1) * (DOG_CHAR_WIDTH)) / 2;
		u8g.drawStr(txt1X, u8g.getHeight() - (DOG_CHAR_HEIGHT) * 3 / 2, STRING_SPLASH_LINE1);
		u8g.drawStr(txt2X, u8g.getHeight() - (DOG_CHAR_HEIGHT) * 1 / 2, STRING_SPLASH_LINE2);
	  #endif
	} while (u8g.nextPage());
	*/
	safe_delay(BOOTSCREEN_TIMEOUT);
}
```