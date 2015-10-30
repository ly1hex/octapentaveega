Attiny85 VGA UART transfer:
===========================

Ascii-codes 32 .. 159 (decimal) are printed on screen as-is.

CR and LF are both treated as same; cursor is placed into
the beginning of next line. If cursor is already on last line, 
screen is scrolled up.

Limited subset of ANSI escapes is supported.

ANSI sequence begins when "Escape" 0x1B (ascii 27 / ESC) character
followed by [ character is encountered.

List of supported ANSI escapes:
-----------------------------------------------------------------------------

Effect | Code


Move cursor location | \<ESC\>[#row#;#column#H	#row# = 0 .. 13
                     | \<ESC\>[#row#;#column#f	#colunn# = 0 .. 31

| Leaving both values undefined moves to (0,0):
| \<ESC\>[H is equal to \<ESC\>[0;0H

| *Moving cursor outside the specified area has undefined effect.*

Clear screen and move cursor to upper left corner (0,0) |	\<ESC\>[2J


Set colors (or reset to		\<ESC\>[#color#;#color#m
default color). Selected
color will stay active until
reset or new color selected.

#color# = color number		Foreground	Background
	black			   30		   40
	red			   31		   41
	green			   32		   42
	yellow			   33		   43
	blue			   34		   44
	magenta			   35		   45
	cyan			   36		   46
	white			   37		   47
	reset to defaults	   0

Color command supports maximum two arguments. Suggested way is to use
value 0 only alone in a command: \<ESC\>[0m resets colors to default.

Example: \<ESC\>[34;47m activates blue text with white background color.

Using other than supported color numbers has undefined effect.
-----------------------------------------------------------------------------
Disable wrap at the end		\<ESC\>[=7l	(lowercase L)
of line (cursor stays
in the last column).
-----------------------------------------------------------------------------
Enable wrap at the end		\<ESC\>[=7h
of line (cursor advances
to next line). Default
mode.
-----------------------------------------------------------------------------

\<ESC\> followed by any other character than '[' stores the character as-is
to screen buffer, allowing to display special characters on screen.

