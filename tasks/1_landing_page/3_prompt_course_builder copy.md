
Something I didn't tell you, this hero section is a big "course generator component".
It is a form presented to user as a carosel. Each step of carosel has 1 input field.
When user answers a question it slides away to the left with animation, and the next question slides out from the right.
Starting from the second question there should be also an option to go back to the prev step.


this is the schema that matches the form:
{
  "topic": string, // text input
  "context": string //text input
  "depth": number //slider with values from 1 to five
} 

Steps of carosel:
1. Course topic:
   Header: What do you want to learn today?
   Sub-header: none
2. Course context:
   Header: Why do you want to lear it?
   Sub-header: Give us some context about how whould you apply this knowledge, so we could adjust course to your needs
3. How deep would you like to dive into the topic?
   Sub-header: none

Above the slider there should be a bold white text saying one of these phrases, depending on the value:
1 - "Just exploring the topic"
2 - "Ready to dip my toes"
3 - "I am up for a solid studying sessh"
4 - "I really want to learn this!"
5 - "I am going to use this professionally"

After submitting the form we will send it to supabase. but lets do that in the next prompt


Regearding the looks, I really like  


---- 
Validation:

When submitting courses, users might want to generate some inappropriate or illegal, harmful content. We need to add validation to prevent that.

I am thinking of:

    In the edge function, before creating a course, call OpenAI's Moderation API (omni-moderation-latest) with the user's input text before processing
    Check the flagged property in the API response. If true, block submission and trigger feedback

    If the content is flagged, provide toast response: "This topic doesn't meet our content guidelines. Please try a different subject."

something like:
// Edge function pseudocode
async function validateContent(inputText) {
  const moderation = await openai.moderations.create({
    input: inputText,
    model: "omni-moderation-latest"
  });
  
  if (moderation.results[0].flagged) {
    throw new Error("CONTENT_VIOLATION"); // Custom error type
  }
  return true;
}

