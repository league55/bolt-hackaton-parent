Now let's do course builder. 
There will be 4 steps. 

1. We already have an input for the first one. 
When user presses start your journey we should save it locally and show input for step 2.
2. One more input asking "Why do you waht to learn it? what is your goal?"
Confirmation button now should say "Almost there ->"
3. A slider with values from 1 to 5. Label explain "How serious are you?". Tooltip explainining "This setting defines how deep will be your course and how difficult will be the accessment"  
This time button should say submit. 

Field names: 
1. topic
2. context
3. depth

Every field should be stored locally, so that on page refresh progress persists unless explicitly reset.

--- 

Perfect. Let's improve slider 
Should be multi-step progress bar (stepper slider) with the following specifications:
There are five equally spaced nodes (steps) aligned horizontally. Each step is an emoji. Between steps there are bars.
The nodes have bars connecting them. The bars between "active" nodes are filled with a blue-to-purple gradient, indicating completed or active steps.
The last active node (step) is visually highlighted as the current step by surrounding it with a ring of evenly spaced purple dots.
Bars between non-active nodea are gray, indicating incomplete steps.
All nodes are of equal size, and the connecting bars are of equal thickness and length.
The overall style is minimal, with smooth gradients and subtle shadows if needed.
This layout should be responsive, in a sense, if it is mobile -> should not be a slider but a simpler more appropriate component.

Slider values are 1 to 5, they much this emoji:
1 - :beach_umbrella:
2 - :nerd_face:
3 - :flexed_biceps:
4 - :face_with_steam:
5 - :exploding_head:

Above the slider there should be a bold white text saying one of these phrases, depending on the value:
1 - "Just exploring the topic"
2 - "Ready to dip my toes"
3 - "I am up for a solid studying sessh"
4 - "I really want to learn this!"
5 - "I am going to use this professionally"

Nothing should be under the slider
