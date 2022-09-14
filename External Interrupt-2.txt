
#include <xc.h>//set PIC configuration bit
__CONFIG(FOSC_HS & WDTE_OFF & PWRTE_OFF & BOREN_OFF & LVP_ON & CPD_OFF & WRT_OFF & CP_OFF);
#define _XTAL_FREQ 20000000// set crystal frequency
unsigned int i;//unsigned int i is a global variable
void blink_red()// function for blink 
{
    for(i=0;i<10;i++)// for loop for blink the red LED 10 times
    {
        PORTCbits.RC2=1;// ON Red LED
        __delay_ms(50);//pause 50 milli second
        PORTCbits.RC2=0;//OFF Red LED
        __delay_ms(50);//pause 50 milli second
    }
}
void interrupt external()//interrupt routine function  
{
    INTCONbits.GIE=0;// set global interrupt to LOW
    if(INTCONbits.INTF == 1)// if external interrupt bit is equal to 1
    {
        PORTBbits.RB2 =0;//set PORTB RB2 to 0
        blink_red();// call the blink function
        
    } 
    INTCONbits.INTF = 0;// clear the external interrupt flag to 0
    INTCONbits.GIE=1;//enable again the global interrupt bit to next interrupt.
}
void main() // main function
{   
    TRISB2 = 0;//set the PORTB RB2 Bit as an output
    TRISC2 = 0;//set the PORTC RC2 Bit as an output
    PORTCbits.RC2=0;// set initial value of RC2 to LOW
    INTCONbits.GIE = 1;//Enable global interrupt
    INTCONbits.PEIE = 1;//Enable Peripheral interrupt 
    INTCONbits.INTE = 1;//Enable  the EXTernal Interrupt Bit RB0 to input
    OPTION_REGbits.INTEDG = 1;//interrupt trigger on raising Edge
    
    while(1)//infinite loop for blink the Yellow LED connected on RB2 
    {
    PORTBbits.RB2 =1;//ON yellow LED
	__delay_ms(500);//pause 500 milli second
	PORTBbits.RB2 =0;//OFF Yellow LED
	__delay_ms(500);//pause 500 milli second
    }
    return ;
}








