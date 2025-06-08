So what we have been calling "content" right now is not really a content. It is more like a proper detailed summary. 
I am attaching an example of syllabus that we get right now. 

Let's implement one more level of "content". We will call it "full_content".
Every topic of module will have corresponding "full_content".
We will use lazy generation. When user opens a course topic they will see a big "Generate topic content" button under topic syllabus content. 
If full content doesn't exist we will schedule it's generation with a new webhook and a new edge function and AI based on topic summary and topic content form syllabus.
User should see a loader in meanwhile. 
Full content is represented in markdown and stored in supabase file storage.
If content was previously generated - users should load it from storage instead of generating. 



Phase 1: Database and Supabase Storage Setup ✅
1. Database Schema
content_items table: Stores all content items with metadata, file paths, and structured data
content_generation_jobs table: Manages AI content generation jobs with retry logic
Proper indexing: For fast content retrieval by course, module, and topic
RLS policies: Secure access control for content management
2. TypeScript Types
Extended the existing Supabase types with new content system interfaces
Added ContentItem, ContentGenerationJob, and related types
Updated CourseWithDetails to include content items
Defined storage bucket constants
3. Content Service
File upload handling: Supports images, videos, audio, documents, and interactive content
Content validation: File size and type validation for different content types
Storage management: Organized file paths and bucket selection
CRUD operations: Create, read, update, delete content items
Content reordering: Manage content order within topics
AI generation: Framework for triggering content generation jobs
4. Storage Buckets Structure
The system is designed to use these Supabase storage buckets:

course-images: For images, diagrams, charts
course-videos: For video content
course-audio: For audio content, podcasts
course-documents: For PDFs, presentations
course-interactive: For interactive content, simulations
5. Key Features
Multi-content support: Text, images, videos, audio, documents, interactive content
Organized storage: Hierarchical file organization by course/module/topic
Metadata support: Flexible JSON metadata for each content item
Order management: Content can be reordered within topics
Generation tracking: Job system for AI-generated content
Security: RLS policies ensure users can only access appropriate content
The database migration is ready to be applied, and the TypeScript interfaces provide a solid foundation for the content management system. The next phases would involve:

Phase 2: Content generation edge functions (AI-powered content creation)
Phase 3: Frontend content management UI
Phase 4: Enhanced learning experience with rich content


## Phase 2:
To implement Phase 2, which focuses on robust content generation considering LLM limitations and citations, you'll need to create a new Supabase Edge Function and a corresponding database trigger. This setup will ensure that when a content generation job is initiated, it's processed asynchronously and reliably.

The Plan
Create supabase/functions/process-content-job/types.ts: You will create a new file to define the TypeScript interfaces for the data structures used by the content generation process, including course configuration, syllabus, content items, and job payloads. This centralizes type definitions for clarity and consistency.

Create supabase/functions/process-content-job/deno.json: You will create a configuration file for Deno, specifying the npm package imports required for the Edge Function, such as supabase-js and zod.

Create supabase/functions/process-content-job/validation.ts: You will add a file containing Zod schemas and a validation function for the AI-generated content. This ensures that the output from the LLM adheres to the expected structure and length constraints before being stored in the database.

Create supabase/functions/process-content-job/database.ts: You will create a service file to encapsulate all database interactions for the content generation function. This includes fetching job details, course configurations, syllabi, creating new content items, and updating job statuses.

Create supabase/functions/process-content-job/ai-generator.ts: You will implement the core logic for interacting with the LLM. This file will construct system and user prompts based on the content type and course depth, send requests to the OpenAI API, and parse the LLM's response. It will also include instructions for the LLM to generate citations.

Create supabase/functions/process-content-job/job-processor.ts: This file will contain the main business logic for processing a content generation job. It will orchestrate fetching job details, validating eligibility, calling the AI generator, validating the generated content, creating the content item in the database, and updating the job status.

Create supabase/functions/process-content-job/request-parser.ts: You will create a utility to parse the incoming request payload for the Edge Function, extracting the job_id and course_configuration_id from either a webhook or a direct function call.

Create supabase/functions/process-content-job/index.ts: This will be the entry point for the new Supabase Edge Function. It will handle incoming HTTP requests, parse the payload, and delegate the job processing to the JobProcessor. It will also include error handling and CORS headers.

Create a new database migration file: You will generate a new SQL migration file (e.g., supabase/migrations/YYYYMMDDHHMMSS_add_content_job_trigger.sql). This file will define a new PostgreSQL function process_content_generation_job and a trigger trigger_process_content_generation_job. This trigger will automatically call the process-content-job Edge Function whenever a new record is inserted into the content_generation_jobs table, ensuring asynchronous processing.


## Phase 3.
Phase 3: Frontend Integration
This phase involves modifying the React application to trigger content generation and display the full content.

Update src/lib/supabase.ts:

Modify the SyllabusTopic interface to include an optional full_content_path?: string property.
Add a new asynchronous function, dbOperations.triggerFullContentGeneration(courseId: string, moduleIndex: number, topicIndex: number), which will:
Insert a new record into the full_content_generation_jobs table.
Invoke the generate-full-content Edge Function using the Supabase client's functions.invoke method, passing the necessary job_id, course_configuration_id, module_index, and topic_index.
Add another asynchronous function, dbOperations.getFullContent(path: string), which will fetch the markdown content from the provided Supabase Storage URL.
Update src/pages/learn-course.tsx:

In the handleTopicSelect function, after determining the currentModule and currentTopic, you will check if currentTopic.full_content_path exists.
If full_content_path exists, call dbOperations.getFullContent to fetch the content and pass it as a prop to the CourseContent component.
If full_content_path does not exist, you will pass a flag to CourseContent indicating that the "Generate topic content" button should be displayed.
You will manage a loading state (isGeneratingFullContent) to show a loader while the content is being generated.
You will implement a mechanism to re-fetch the syllabus or the specific topic's data after a generation job is triggered, to update the UI once the full_content_path is available.
Update src/components/course/course-content.tsx:

Modify the component to accept new props: fullContent?: string, onGenerateFullContent: () => void, and isGeneratingFullContent: boolean.
Conditionally render the content:
If fullContent is provided, display this detailed markdown content.
If isGeneratingFullContent is true, display a loading indicator.
Otherwise (if fullContent is not available and not currently generating), display the "Generate topic content" button.
The existing topic.content (detailed summary) should remain visible, positioned above the new "full_content" or the "Generate" button as per the requirement.


