# Commented disassemblies of IBM keyboard device controller (8042) and IBM Keyboard (8048) ROM-s

A long, long time ago I analyzed and commented IBM's 8042 AT controller ROM to see how it worked (see [8042_1503033.TXT](8042_1503033.TXT) and [8042_INTERN.TXT](8042_INTERN.TXT)). Since then some new ROM dumps appeared in the MAME project - I don't check it very often - but was very happy to see the long awaited <a href="8042_PS2_INTERN.TEXT">8042 PS/2 controller on the list: 72X8455</a>. So I revived my UPI disassembler and started to look at it.. Then only out of curiosity I was wondering what those IBM keyboards did on the other end: they have an Intel 8048 microcomputer inside, which is very similar to the 8042 UPI, so lets look at the ROM code and the internal operation how these keyboards worked too. I've found 2 IBM keyboard ROM dumps in the MAME project, which looked like from an 83-key XT and an 84-key AT Model F. 

This table shows some features of these keyboards, roughly with IBM PC models in line and links to the four commented disassemblies: 






| IBM Keyboard   | Capacitive |  Schematics<br>IBM TechRef |  Controller/<br>1K ROM dump | SENSE<br>AMPLIFIER | Keyboard<br>Matrix = N  |  PC Controller/<br>2K ROM dump    |
|----------------|------------|--------------|--------------|------------|----------------|-----------------|
| 83-Key Type-1<br>Model F |  Yes |  Yes |   i8048<br>none     |  4-sense<br>5119699<br>IBM 14 |  24 x 4 =  96  |    BIOS+LS322<br>N/A<br><br>IBM PC 1981    |
| 83-Key Type-2<br>Model F  |    Yes     |     Yes      |   i8048<br><a href="8048_XT_INTERN.TEXT">4584751</a>     |  8-sense<br>8273565<br>IBM 9314   |  12 x 8 =  96  |    BIOS+LS322<br>N/A<br><br>IBM PC/XT 1983   |
| 84-Key AT<br>Model F      |    Yes     |     No<br>(kbdbabel.org)       |   i8048<br><a href="8048_AT_INTERN.TEXT">1503099</a>      |  8-sense<br>6014810<br>IBM 9314   |  16 x 8 = 128  |    i8042 AT<br><a href="8042_1503033.TXT">1503033</a><br><br>IBM PC/AT 1984     |
| 101/102-Key<br>Model M    |    No      |     Yes      |   M6805<br>none      |  N/A       |  16 x 8 = 128  |    i8042 PS/2<br><a href="8042_PS2_INTERN.TEXT">72X8455</a><br><br>IBM PS/2 1987   |
