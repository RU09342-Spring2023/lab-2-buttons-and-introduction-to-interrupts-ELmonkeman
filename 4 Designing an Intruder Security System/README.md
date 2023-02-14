/*
 * OccupancyDetector.c
 *
 *  Created on: Feb 6, 2023
 *      Author: David Miller
 */
#include <msp430.h>
#include <time.h>
#define ARMED_STATE 0
#define WARNING_STATE 1
#define ALERT_STATE 2
int main(void)
{
        P1OUT &= ~BIT0;
        P1DIR |=BIT0;
        PM5CTL0 &=~LOCKLPM5;
        WDTCTL = WDTPW | WDTHOLD;   // stop watchdog timer
        P6OUT &= ~BIT6;
        P6DIR |=BIT6;
        PM5CTL0 &=~LOCKLPM5;
        P4REN |= BIT1;
        P4OUT |=BIT1;
        P6OUT ^=BIT6;

char state = WARNING_STATE;
int y = 0;
int x = 0;
while(1)
{
  switch (state) {
    case WARNING_STATE:
    {
        while (!(P4IN & BIT1))
        {
            P1OUT^=BIT0;
            P6OUT &= ~BIT6;
                if (x > 19)
                {
                y = 1;
                break;
                }
            x++;
            __delay_cycles(500000);
        }
    }
    case ALERT_STATE:
    {
        while ( y > 0)
            {
            P1OUT|=BIT0;
            P6OUT &= ~BIT6;
                   if(!(P4IN & BIT1)==0x00)
                    {
                    __delay_cycles(500000);
                    break;
                    }
            }
    }
    default : //ARMED_STATE
    {
        while (1)
            {
            x=0;
            P6OUT^=BIT6;
            P1OUT &= ~BIT0;
            __delay_cycles(3000000);
                if (!(P4IN & BIT1))
                    {
            break;
        }
  }
  }

}
}
}
