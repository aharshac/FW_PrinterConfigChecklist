# HB: ultralcd.cpp


## Instructions
1. Add **Quick Bed Leveling Options** to `void lcd_prepare_menu()`.
2. Move `void lcd_prepare_menu()` between `void lcd_move_menu()` and **Control submenu** section
3. Copy **Quick Bed Leveling Screens** section before the new position of `void lcd_prepare_menu()`
4. Modify **Mesh Bed Leveling** to support **Z_MAX_ENDSTOP**

## Code
### **Bed Leveling Options**
```clike

// //
// //Quick Bed Leveling
// //
MENU_ITEM(submenu, "FW Bed Leveling Wizard", bedLevelingScreen1);
MENU_MULTIPLIER_ITEM_EDIT(float32, "FW Z Offset", &home_offset[Z_AXIS], -20, 20);

```

&nbsp;

### **Quick Bed Leveling Screens**
```clike

// LCD Move menu
// void lcd_move_menu()


/**
*
* Quick Bed Leveling Screens
*
*/


// void _lcd_move_xyz(const char* name, AxisEnum axis) {
//   if (lcd_clicked) { return lcd_goto_previous_menu(); }
//   ENCODER_DIRECTION_NORMAL();
//   if (encoderPosition) {
//     refresh_cmd_timeout();

//     float min = current_position[axis] - 1000,
//           max = current_position[axis] + 1000;


//     // Get the new position
//     current_position[axis] += float((int32_t)encoderPosition) * move_menu_scale;


//     // Limit only when trying to move towards the limit
//     if ((int32_t)encoderPosition < 0) NOLESS(current_position[axis], min);
//     if ((int32_t)encoderPosition > 0) NOMORE(current_position[axis], max);

//     manual_move_to_current(axis);

//     encoderPosition = 0;
//     lcdDrawUpdate = LCDVIEW_REDRAW_NOW;
//   }
//   if (lcdDrawUpdate) lcd_implementation_drawedit(name, ftostr41sign(current_position[axis]));
// }

void bedLevelingDone() {
	lcd_goto_previous_menu();
	lcd_completion_feedback();
	defer_return_to_status = false;
	home_offset[Z_AXIS] = -current_position[Z_AXIS] ;
	settings.save();
	//save Z offset
}



void setZOffsetMessage(){
	u8g.drawStr(0,12,"Slide a paper sheet");
	u8g.drawStr(0,24,"under Nozzle while");
	u8g.drawStr(0,36,"turning dial, untill");
	u8g.drawStr(0,48,"paper stops sliding");

	if (lcd_clicked) { 
		return bedLevelingDone(); 
	}
        
	ENCODER_DIRECTION_NORMAL();
		
	if (encoderPosition) {
		refresh_cmd_timeout();

		// Get the new position
		current_position[Z_AXIS] += float((int32_t)encoderPosition) * 0.05;

		manual_move_to_current(Z_AXIS);

		encoderPosition = 0;
	}
}


void setZOffsetMove() {
	lcd_goto_screen(setZOffsetMessage);
	enqueue_and_echo_commands_P(PSTR("G1 Z20"));
	enqueue_and_echo_commands_P(PSTR("G1 X100 Y100 Z2"));
}



void bedLevelingThirdPositionMessage() {
	u8g.drawStr(0,12,"Lock back leveling");
	u8g.drawStr(0,24,"knob clockwise");
	u8g.drawStr(0,36,"dial");

	if (lcd_clicked) {
		setZOffsetMove();
	}
}

void bedLevelingThirdPosition() {
	lcd_goto_screen(bedLevelingThirdPositionMessage);
	enqueue_and_echo_commands_P(PSTR("G1 Z20"));
	enqueue_and_echo_commands_P(PSTR("G1 X110 Y197"));
	enqueue_and_echo_commands_P(PSTR("G1 X110 Y197 Z0.05"));
}



void bedLevelingSecondPositionMessage() {
	u8g.drawStr(0,12,"Lock left leveling");
	u8g.drawStr(0,24,"knob clockwise");
	u8g.drawStr(0,36,"and click the dial");

	if (lcd_clicked) {
		bedLevelingThirdPosition();
	}
}

void bedLevelingSecondPosition() {
	lcd_goto_screen(bedLevelingSecondPositionMessage);
	enqueue_and_echo_commands_P(PSTR("G1 Z20"));
	enqueue_and_echo_commands_P(PSTR("G1 X35.6 Y21"));
	enqueue_and_echo_commands_P(PSTR("G1 X35.6 Y21 Z0.05"));
}


void bedLevelingFirstPositionMessage() {
	u8g.drawStr(0,12,"Lock right leveling");
	u8g.drawStr(0,24,"knob clockwise");
	u8g.drawStr(0,36,"and click the dial");

	if (lcd_clicked) {
		bedLevelingSecondPosition();
	}
}

void bedLevelingFirstPosition() {
	lcd_goto_screen(bedLevelingFirstPositionMessage);
	enqueue_and_echo_commands_P(PSTR("G1 X181 Y21"));
	enqueue_and_echo_commands_P(PSTR("G1 X181 Y21 Z0"));
}

void bedLevelingUnlockMessage() {
	u8g.drawStr(0,12,"Unlock all leveling");
	u8g.drawStr(0,24,"knobs anti-clockwise");
	u8g.drawStr(0,36,"and click the dial");

	if (lcd_clicked) {
		bedLevelingFirstPosition();
	}
}

 /**
 * Step 4: Display "Click to Begin", wait for click
 *         Move to the first probe position
 */
void bedLevelUnlockPosition() {
	lcd_goto_screen(bedLevelingUnlockMessage);
	enqueue_and_echo_commands_P(PSTR("G1 X100 Y100 Z50"));
}

/**
 * Step 4: Display "Click to Begin", wait for click
 *         Move to the first probe position
 */
void bedLevelingHomingDone() {
	if (lcdDrawUpdate) 
		lcd_implementation_drawedit(PSTR(MSG_LEVEL_BED_WAITING));
	if (lcd_clicked) {
		bedLevelUnlockPosition();
	}

}

/**
* Step 2: Display "Homing XYZ" - Wait for homing to finish
*/
void bedLevelingHome() {
	if (lcdDrawUpdate) lcd_implementation_drawedit(PSTR(MSG_LEVEL_BED_HOMING), NULL);
	lcdDrawUpdate = LCDVIEW_CALL_NO_REDRAW;
	if (axis_homed[X_AXIS] && axis_homed[Y_AXIS] && axis_homed[Z_AXIS])
	lcd_goto_screen(bedLevelingHomingDone);
}

/**
* Step 1: Start Bed leveling by homing bed
*/
void bedLevelingScreen1() {
	defer_return_to_status = true;
	//tempZHomeOffset = 0 ;
	//tempZHomeOffset = home_offset[Z_AXIS] ;
	home_offset[Z_AXIS] = 0;
	axis_homed[X_AXIS] = axis_homed[Y_AXIS] = axis_homed[Z_AXIS] = false;
	lcd_goto_screen(bedLevelingHome);
	enqueue_and_echo_commands_P(PSTR("G28"));
}


// "Control" submenu

```

&nbsp;


### **Mesh Bed Leveling**
1. Create `float temp_z_home_offset` variable.
```clike

  #if ENABLED(LCD_BED_LEVELING)

    /**
     *
     * "Prepare" > "Level Bed" handlers
     *
     */

    static uint8_t manual_probe_index;
	float temp_z_home_offset;

```

2. Move `home_offset[Z_AXIS]` to temp variable after homing.
```clike

/**
 * Step 4: Display "Click to Begin", wait for click
 *         Move to the first probe position
 */
void _lcd_level_bed_homing_done() {
  if (lcdDrawUpdate) lcd_implementation_drawedit(PSTR(MSG_LEVEL_BED_WAITING));
  if (lcd_clicked) {
	temp_z_home_offset = home_offset[Z_AXIS];
	home_offset[Z_AXIS] = 0;
	
```

3. Set `home_offset[Z_AXIS]` from temp variable.
```clike

//
// Bed leveling is done. Wait for G29 to complete.
// A flag is used so that this can release control
// and allow the command queue to be processed.
//
// When G29 finishes the last move:
// - Raise Z to the "manual probe height"
// - Don't return until done.
//
// ** This blocks the command queue! **
//
void _lcd_level_bed_done() {
  if (!lcd_wait_for_move) {
	#if MANUAL_PROBE_HEIGHT > 0 && DISABLED(MESH_BED_LEVELING)
	  // Display "Done" screen and wait for moves to complete
	  line_to_z(Z_MIN_POS + MANUAL_PROBE_HEIGHT);
	  lcd_synchronize(PSTR(MSG_LEVEL_BED_DONE));
	#endif
	lcd_goto_previous_menu();
	lcd_completion_feedback();
	defer_return_to_status = false;
  }
  if (lcdDrawUpdate) lcd_implementation_drawmenu_static(LCD_HEIGHT >= 4 ? 1 : 0, PSTR(MSG_LEVEL_BED_DONE));
  home_offset[Z_AXIS] = temp_z_home_offset;
  lcdDrawUpdate = LCDVIEW_CALL_REDRAW_NEXT;
}
	
```