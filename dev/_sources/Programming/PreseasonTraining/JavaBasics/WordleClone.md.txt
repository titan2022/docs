# 2 - Wordle Clone

2024 September 11, 1.5 hours

[Slideshow](https://docs.google.com/presentation/d/1uhjIZ65J4VvHE7sGBRPcb_pXgYdxOnFbFID_cAryhpg/edit#slide=id.p)

We split up the team into two groups. The people who were new to Java worked on the Wordle clone. The people who were more familiar worked on the Brainf interpreter.

## Final code

```{code-block} java
:caption: Main.java
:lineno-start: 1

import java.util.ArrayList;
import java.util.Scanner;

public class Main {
    private static final int WORD_LENGTH = 3;
    private static final int MAX_ATTEMPTS = 9999999;

    public static void main(String[] args) {
        WordleReader reader = new WordleReader();
        String secretWord = reader.getRandomWord();
        int attempts = 0;

        Scanner inputScanner = new Scanner(System.in);

        boolean isRunning = true;  
		while (isRunning) {
            String userInput = inputScanner.nextLine();

            if (userInput.equals(secretWord)) {
                System.out.println("Congrats you guessed it !!! " + secretWord);
                isRunning = false;
                break;
            }

            attempts++;
            if (attempts > MAX_ATTEMPTS) {
                System.out.println("Out of attempts :(");
                isRunning = false;
                break;
            }

            String result = "";
            ArrayList<Integer> matchingIndeces = new ArrayList<>();
            for (int i = 0; i < WORD_LENGTH; i++) {
                char letter = userInput.charAt(i);

                if (letter == secretWord.charAt(i)) {
                    result += reader.turnGreen(letter);
                    matchingIndeces.add(i);
                } else if (secretWord.indexOf(letter) == -1) {
                    result += letter;
                } else if (!matchingIndeces.contains(secretWord.indexOf(letter))) {
                    result += reader.turnYellow(letter);
                } else {
                    result += letter;
                }
            }

            System.out.println(result);
        }
    }
}
```

```{code-block} java
:caption: WordleReader.java
:lineno-start: 1

import java.util.ArrayList;
import java.util.Random;
import java.io.*;

public class WordleReader {
    private ArrayList<String> words = new ArrayList<String>();
    private Random randGen = new Random(System.currentTimeMillis());

    public WordleReader() {
        try {
            FileInputStream fstream = new FileInputStream("words.txt");

            DataInputStream in = new DataInputStream(fstream);
            BufferedReader br = new BufferedReader(new InputStreamReader(in));

            String strLine;

            while ((strLine = br.readLine()) != null) {
                words.add(strLine);
            }

            in.close();
        } catch (Exception err) {
            System.err.println(err);
        }
    }

    public String getRandomWord() {
        return words.get(randGen.nextInt(words.size())).toLowerCase();
    }

    public boolean wordExists(String word) {
        return words.contains(word);
    }
    
    public String turnGreen(String text) {
        return "\e[42m" + text + "\e[0m";
    }

    public String turnYellow(String text) {
        return "\e[43m" + text + "\e[0m";
    }
    
    public String turnGreen(char text) {
        return "\e[42m" + text + "\e[0m";
    }

    public String turnYellow(char text) {
        return "\e[43m" + text + "\e[0m";
    }
}
```
