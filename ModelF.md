## Model F keyboards and the 8048

IBM made something remarkable in 1981: a capacitive matrix keyboard with buckled spring buttons coupled to a custom made analogue Sense Amplifier IC, all under the control of the Intel 8048 microcomputer. These Model F keyboards were a big success accompanied the IBM PC/XT/AT computers (not to mention Model M, the father of all modern keyboard layouts). All three Model F keyboards are based on capacitive sensing: 

	
	                                        SENSE AMPLIFIER
	                                       (see US 4,305,135)
	           M                 _   _
	     +++++++++++++-           | |                        +------+               
	     +++++++++++++-           |_|           +----+       |      |               
	   N +++++++++++++- ----------------------> | SA | ----> |      |               
	     +++++++++++++-          sense          +----+       | 8048 |   --> CLOCK/DATA
	     +++++++++++++-                                      |      |               
	     +++++++++++++-                                      |      |       serial transmission            
	     |||||||||||||                                       |      |       of make- and breakcodes
	     +++++++++++++-------------------<------------------ |      |                                                
	                             drive                       +------+
	  sandwich matrix of                                                
	   capacitive pads                                                  
	 Capacitive Circuitboard                           
	   (see US 3,921,167)

The Model F keyboard has capacitive switches, each button is a variable capacitor. There is a custom made, analogue IC on the controller board, the silver-shielded SENSE AMPLIFIER (SA), which can measure capacitance and decides whether the button is released or depressed (i.e. actuated). Unfortunately, this IC is undocumented, but there are some clues in several US patents filed by IBM in the '70s how it might work and what it does. 

Just briefly, keys are located in a N x M keyboard matrix with M drive and N sense lines. The 8048 scans the matrix for each key by selecting and pulsing one drive line, while selecting and reading the state of one sense line at a time, sampling the SA's output pin. 

Here are some notes on the SA after searching the internet for information, reading US patents, etc. I even went through a few handbooks from Signetics, Analog Devices from the '70s- early '80s, but without any luck to find anything similar to it among consumer analog IC products. Probably because I didn't even know what to look for..



### The capacitive circuitboard 

In my interpretation this is how the IBM Model F works with capacitive technology. Most of the information here comes from US 3,921,167 CAPACITIVE CIRCUITBOARD, filed in 1974. There is a two-sided capacitive circuitboard. Under each button there are pairs of conductive pads on both sides: one for drive and one for sense (and one stands alone). The vertically moving capacitive coupling plate is 'buckled down' by a keypress. 

	                  BUTTON                    
	                    |
	                    |                           
	           ###################
	            C2             C3   .----> sense    
	                                |              
	       -----------     -----------
	  ||||||||||||||||||||||||||||||||||||||||||||||||||||||||
	  ||||||||| C1 |||||||||||||||||||||||||||||||| board ||||
	  ||||||||||||||||||||||||||||||||||||||||||||||||||||||||
	       -----------                
	            |                                                   
	drive ->----´                                                   

Changing voltage on the drive pad is "picket up" by the sense pad due to capacitive coupling between these components, and it is different when the coupling plate is close or further away from the board. According to IBM the capacitance of C1 is constant and is around 45 pF. Capacitance C2 and C3 are variable: when the plate is down appx. 11pF, when up it drops below 1pF. C1, C2 and C3 are in series and computing the net capacitance gives these two results:  

	drive ->----||----||----||----> sense       4.9 pF   vs.  0.49 pF
	            C1    C2    C3                   DOWN          UP

Amplifiers can sense current of charging/discharging as little as 1pF capacitors (we are in the '70s..), and this is how the sense amplifier part works. As a background, lets see what happens with the current - ideally - when voltage is applied to a capacitor. A capacitor responds to voltage change by either charge or discharge. The greater the capacitance, the greater charge/discharge current it will be. Simplified, by applying the same voltage on two capacitors with different capacitance (C2>C1), there will be current on the "other side" due to capacitive coupling of the plates: 

	            ___________
	           |           |                                 
	           |           |                                  
	V _________|           |________
	                                            
	            _                                         
	           | |                     
	I1_________| |_________   ______                I1  ||    I1    
	                       | |                V --------||--------       Key is released 
	                       |_|                          ||          
	                                                   C = .5pF     
	            _                                                   
	           | |                                                            
	           | |                                                            
	           | |                                                            
	I2_________| |_________   _______               I2  ||    I2              
	                       | |                V --------||--------       Key is depressed: greater capacitive coupling
	                       | |                          ||                     
	                       | |                         C = 4.5pF               
	                       |_|                     


Applying voltage "pushes" electrons on one plate, very rapidly, which "drives" electrons away from the other plate, also very rapidly. When the capacitor is charged the current stops and falls back to zero (this is slower and more like an exponential curve). That appears as a current "peak". When voltage falls, current flows in the opposite direction (discharge). With an analogue circuit we can detect the current "peak" on the other side of the capacitor and based on its amplitude make a decision whether the key is depressed or not. 

There is only one problem: noise. Or in this case stray capacitance: when conductive elements are close to each other there always exists some capacitive coupling between all elements, voltage/current change is slightly 'picked up' by neighbouring elements, which is parasitic, not wanted and may disturb operation. Here we are talking about measuring uA-s and pF-s and this will be an issue. Not to mention the effect of 50/60Hz AC-sources or static discharge of the operator, etc. Another issue is that there is a significant difference in capacitance of each physical key location on the board, each key deviates slightly +/- from the ideal .5/4.5pF. 

	          *        *      PLUS individual key   
	      *       *  *   *    stray capacitance
	  ---*--*------**-*---------------------------------  some "overall"        ---->   SA
	  **     *   *      *     MINUS individual key        stray capacitance
	    *       *             stray capacitance  
	                          
	0 --------------------------------------------------  ideal 

To minimize stray capacitance first, the whole assembly is heavily shielded by grounded metal plates. Second, according to IBM's invention, the drive plates on the bottom side are surrounded by a ground shield to avoid 'cross-driving' of other pads. The SA has adjustable variable threshold (patented) to detect coupling. The overall stray capacitance is tuned by a reference capacitor on the PCB (Cf?) with threshold 0, while each individual key with variable threshold between 1..4 (84-key). Based on the ROM code in the 83-key Type-2, this threshold value is 3 and used for all keys. 

This again, doesn't prevent sensing a false key depression introduced by random noise and unwanted current peak. Therefore the ROM code uses double (83-key) and even triple (84-key) detection pulses on the drive line, and throws away results, which are different. 


