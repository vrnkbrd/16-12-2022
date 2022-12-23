# 16-12-2022
### TASK 6kyu
You have been hired by a company making electric garage doors. Accidents with the present product line have resulted in numerous damaged cars, broken limbs and several killed pets. Your mission is to write a safer version of their controller software.
Specification
We always start with a closed door. The remote control has exactly one button, with the following behaviour.
If the door is closed, a push starts opening the door, and vice-versa
It takes 5 seconds for the door to open or close completely
While the door is moving, one push pauses movement, another push resumes movement in the same direction
In order to make the door safer, it has been equiped with resistance-based obstacle detection. When the door detects an obstacle, it must immediately reverse the direction of movement.
Input
A string where each character represents one second, with the following possible values.
'.' No event
'P' Button has been pressed
'O' Obstacle has been detected (supersedes P)
As an example, '..P....' means that nothing happens for two seconds, then the button is pressed, then no further events.
Output
A string where each character represents one second and indicates the position of the door (0 if fully closed and 5 fully open). The door starts moving immediately, hence its position changes at the same second as the event.
Example
..P...O..... as input should yield 001234321000 as output

#### My solution
```Java
public class Door {
    public static String run(String events) {
    
        int state = 0, dir = 1;
        boolean moving = false;
        StringBuilder out = new StringBuilder();
        
        for (int n = 0 ; n < events.length() ; n++) {
            char c = events.charAt(n);
            
            if (c == 'O')         dir *= -1;
            else if (c == 'P')    moving = !moving;
            if (moving)           state += dir;
            if (state % 5 == 0) {
                moving = false;
                dir = state == 0 ? 1 : -1;
            }
            out.append(state);
        }
        return out.toString();
    }
}
```
### Fav solution
```Java
public class Door {

  public static String run(String events) {
    int dir = 0, pos = 0; 
    boolean paused = false;
    String s = "";
    
    for (final char event : events.toCharArray()) {
      if (event == 'P') { if (pos == 0) dir = 1; else if (pos == 5) dir = -1; else paused = !paused; } 
      else if (event == 'O') dir = -dir;      
      // Unless paused, the door moves in direction dir (keeping in range 0-5)
      s += pos = Math.max(0, Math.min(pos += paused ? 0 : dir, 5)); 
    }
    
    return s;
  }
}
```

### TASK 6 kyu
DESCRIPTION:
Your friend won't stop texting his girlfriend. It's all he does. All day. Seriously. The texts are so mushy too! The whole situation just makes you feel ill. Being the wonderful friend that you are, you hatch an evil plot. While he's sleeping, you take his phone and change the autocorrect options so that every time he types "you" or "u" it gets changed to "your sister."
Write a function called autocorrect that takes a string and replaces all instances of "you" or "u" (not case sensitive) with "your sister" (always lower case).
Return the resulting string.
Here's the slightly tricky part: These are text messages, so there are different forms of "you" and "u".
For the purposes of this kata, here's what you need to support:
"youuuuu" with any number of u characters tacked onto the end
"u" at the beginning, middle, or end of a string, but NOT part of a word
"you" but NOT as part of another word like youtube or bayou

### My solution 
```Java
public class Kata {
  public static String autocorrect(String input) {
    return input.replaceAll("\\b(?i:(you+)|u)\\b","your sister");
  }
}
```
### Fav solution
```Java
public class Kata {
  public static String autocorrect(String input) {
    return input.replaceAll("(?i)\\b(u|you+)\\b", "your sister");
  }
}
It s literally the same but marked as best practice
```
