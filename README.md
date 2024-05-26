# Commented disassemblies of IBM keyboard device controller (8042) and IBM Keyboard (8048) ROM-s

A long, long time ago I analyzed and commented IBM's 8042 AT controller ROM to see how it worked (see [8042_1503033.TXT](8042_1503033.TXT) and [8042_INTERN.TXT](8042_INTERN.TXT)). Since then some new ROM dumps appeared in the MAME project - I don't check it very often - but was very happy to see the long awaited <a href="8042_PS2_INTERN.TEXT">8042 PS/2 controller on the list: 72X8455</a>. So I revived my UPI disassembler and started to look at it.. Then only out of curiosity I was wondering what those IBM keyboards did on the other end: they have an Intel 8048 microcomputer inside, which is very similar to the 8042 UPI, so lets look at the ROM code and the internal operation how these keyboards worked too. I've found 2 IBM keyboard ROM dumps in the MAME project, which looked like from an 83-key XT and an 84-key AT Model F. 

This table shows some features of these keyboards, roughly with IBM PC models in line and links to the four commented disassemblies: 

<pre>
|----------------|------------|--------------|--------------|------------|----------------|-----------------|
| IBM Keyboard   | Capacitive |  Schematics  |  Controller/ |   SENSE    |  Keyboard      |  PC Controller/ |
|                |            |  IBM TechRef |  1K ROM dump |  AMPLIFIER |  Matrix =   N  |  2K ROM dump    |
|----------------|------------|--------------|--------------|------------|----------------|-----------------|
| 83-Key Type-1  |    Yes     |     Yes      |   i8048      |  4-sense   |  24 x 4 =  96  |    BIOS+LS322   |
| Model F        |            |              |   none       |  5119699   |                |    N/A          |
|                |            |              |              |  IBM 14    |                |                 |
|                |            |              |              |            |                |  IBM PC 1981    |
|----------------|------------|--------------|--------------|------------|----------------|-----------------|
| 83-Key Type-2  |    Yes     |     Yes      |   i8048      |  8-sense   |  12 x 8 =  96  |    BIOS+LS322   |
| Model F        |            |              |   <a href="8048_XT_INTERN.TEXT">4584751</a>    |  8273565   |                |    N/A          |
|                |            |              |              |  IBM 9314  |                |                 |
|                |            |              |              |            |                |  IBM PC/XT 1983 |
|----------------|------------|--------------|--------------|------------|----------------|-----------------|
| 84-Key AT      |    Yes     |     No       |   i8048      |  8-sense   |  16 x 8 = 128  |    i8042 AT     |
| Model F        |            |(kbdbabel.org)|   <a href="8048_AT_INTERN.TEXT">1503099</a>    |  6014810   |                |    <a href="8042_1503033.TXT">1503033</a>      |
|                |            |              |              |  IBM 9314  |                |                 |
|                |            |              |              |            |                |  IBM PC/AT 1984 |
|----------------|------------|--------------|--------------|------------|----------------|-----------------|
| 101/102-Key    |    No      |     Yes      |   M6805      |  N/A       |  16 x 8 = 128  |    i8042 PS/2   |
| Model M        |            |              |   none       |            |                |    <a href="8042_PS2_INTERN.TEXT">72X8455</a>      |
|                |            |              |              |            |                |                 |
|                |            |              |              |            |                |   IBM PS/2 1987 |
|----------------|------------|--------------|--------------|------------|----------------|-----------------|
</pre>


