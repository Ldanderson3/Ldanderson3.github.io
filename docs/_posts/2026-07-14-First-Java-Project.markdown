---
layout: post
title:  "First Java Project - The Number Game"
date:   2026-07-14 15:39:00 -0600
categories: Java
---

## Java Syntax
Since the last post, I have Been learning the basics of java syntax, which varies from python greatly. After spending some time I have started to get the basics of it, and to test my skill I decided to make a simple game, one I have made in python before. 


## The Number Game
This game is simple, a number is chosen by the computer and the player has to guess it. Aftr each guess, the computer responds with too low or too high, telling the player how to get closer to the number, once the number is guessed, the computer informs the player that the game is over, and thier guess was correct.

## Random 
As I am not yet aware of how to do random number generation, the number in the game is set at 10.

## Code
```
package Java_First_Project;
import java.util.Scanner;

public class Java_First_Project {
    public static void main(String[] args) {
        int number = 10;
        Scanner scanner = new Scanner(System.in);
        System.out.println("This is an easy number guessing game\nType a number and I will reply with 'Too high' or 'Too low'\n Guess a number to start");
        String guess = scanner.nextLine();
        int int_guess = Integer.parseInt(guess);
        while (int_guess != number) {
            if (int_guess < number) {
                System.out.println("Too low");
            }
            else {
                System.out.println("Too High");
            }
            System.out.println("Enter your next guess");
            guess = scanner.nextLine();
            int_guess = Integer.parseInt(guess);
        }
        System.out.println("Congrats! You've guessed the number!");

        scanner.close();
    }
}
```