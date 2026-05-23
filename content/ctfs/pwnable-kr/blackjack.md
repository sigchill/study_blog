+++
date = '2025-12-19T18:26:37+02:00'
draft = false
title = 'Blackjack'
+++

# Blackjack

This is a simple challenge, no memory corruption needed here,
we receive a program witha  source code in c that implements blackjack
the goal here is to reach 1,000,000 points.

however playing the game manually is way too slow and nighly impossible to
reach such goal, however when looking at the source code we see

```c

int betting() //Asks user amount to bet
{
 printf("\n\nEnter Bet: $");
 scanf("%d", &bet);
 
 if (bet > cash) //If player tries to bet more money than player has
 {
        printf("\nYou cannot bet more money than you have.");
        printf("\nEnter Bet: ");
        scanf("%d", &bet);
        return bet;
 }
 else return bet;
}
```

There is no check for negative values ,
and in the main game loop there is this code snippet

```c
 if(p>21) //If player total is over 21, loss
         {
             printf("\nWoah Buddy, You Went WAY over.\n");
             loss = loss+1;
             cash = cash - bet;
             printf("\nYou have %d Wins and %d Losses. Awesome!\n", won, loss);
             dealer_total=0;
             askover();
         }
 ```
So if we bet a negative value larger than 1,000,000 and we lose
we can win instantly because cash - (-100000000) is cash + 100000000 which is exactly 
what we need
