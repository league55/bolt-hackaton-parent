Please add a page that would serve as a course generator.

It should have following fields:
  - [] Course Topic input field (what do you want to learn?), required
  - [] Goal text field, up tp 200 characters (What is your goal? Why do you want to learn it?)
  - [] Depth selector (1-5) (How deep do you want to dive into it?)
  - [] Resources: up to 3 links to some open resources that contain base information

Until the user starts editing, text fields should have typewriter effect that fills them with examples. 

Some examples:
```
CourseConfigurations: [
{
    "Topic":"Sentry",
    "Goal":"To understand main functionality and why do I need it",
    "Depth"": 2, 
    "Resources":["https://docs.sentry.io/"],
},
{
    "Topic":"Stoicism",
    "Goal":"To shine on a dinner with my friends tonigt",
    "Depth"": 2, 
    "Resources":["https://en.wikipedia.org/wiki/Stoicism", "https://plato.stanford.edu/entries/stoicism/"],
},
{
    "Topic":"Python Basics",
    "Goal":"To be able to write a basic ML script for work",
    "Depth"": 3, 
    "Resources":[],
}
]
```

Make it as a three step carousel form, in a card with use bold types and catchy anymations. We really need to keep users entertained while they are filling it. 
Please maintain current look and feel. 
Make it a notch lighter themed then the main page. 

In future, when user hits submit, it will be calling supabase edge function. 
For now, let's make a stub with a beatiful loader in Orion Path / stars / cosmos theame.