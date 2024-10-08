# 3 - Brainf interpreter

2024 September 11, 1.5 hours

[Slideshow](https://docs.google.com/presentation/d/1uhjIZ65J4VvHE7sGBRPcb_pXgYdxOnFbFID_cAryhpg/edit#slide=id.p)

We split up the team into two groups. The people who were new to Java worked on the Wordle clone. The people who were more familiar worked on the Brainf interpreter.

## Final code

```{code-block} java
:caption: Main.java
:lineno-start: 1

public class Main {
    public static void main(String[] args) {
        BrainfuckReader reader = new BrainfuckReader();
        String code = reader.getCode("code.brainf");
        BrainfuckInterpreter interpreter = new BrainfuckInterpreter(code);
        interpreter.interpret();
    }
}
```

```{code-block} java
:caption: BrainfuckReader.java
:lineno-start: 1

import java.io.File;
import java.nio.file.Files;

public class BrainfuckReader {
    public String getCode(String fileName) {
        try {
            return Files.readString(new File(fileName).toPath());
        } catch (Exception err) {
            System.err.println(err);
        }
        return "";
    }
}
```

```{code-block} java
:caption: BrainfuckInterpreter.java
:lineno-start: 1

public class BrainfuckInterpreter {
    String code;
    char[] memory = new char[3000];
    int[] posRightBracketForLeftBracket = new int[3000];

    public BrainfuckInterpreter(String code) {
        this.code = code;
    }

    public void interpret() {
        int dataPointer = 0;
        int posLeftBracket = -1;
        for(int i = 0; i < this.code.length(); i++) {
            char command = this.code.charAt(i);

            switch (command) {
                case '[': {
                    posLeftBracket = i;
                } break;
                case ']': {
                    posRightBracketForLeftBracket[posLeftBracket] = i;
                }
                default: break;
            }
        }
        posLeftBracket = -1;
        for(int i = 0; i < this.code.length(); i++) {
            char command = this.code.charAt(i);

            switch (command) {
                case '>':
                    dataPointer++;
                    break;
                case '<':
                    dataPointer--;
                    break;
                case '+':
                    memory[dataPointer]++;
                    break;
                case '-':
                    memory[dataPointer]--;
                    break;
                case '.': {
                    System.out.print(memory[dataPointer]);
                } break;
                case '[': {
                    posLeftBracket = i;
                    if(memory[dataPointer] == 0) {
                        i = posRightBracketForLeftBracket[i];
                    }
                } break;
                case ']': {
                    if(memory[dataPointer] != 0) {
                        i = posLeftBracket;
                    }
                } break;
                default:
                    break;
            }
        }
    }
}
```
