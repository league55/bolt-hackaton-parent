To initiate a conversation with Tavus's Conversational Video Interface (CVI), you need to create a conversation using the Create Conversation API endpoint. 
 
Step 1: Create a Conversation
Use the POST /v2/conversations endpoint to initiate a conversation:

Endpoint: https://tavusapi.com/v2/conversations

Required Headers:

x-api-key: YOUR_API_KEY
Basic Request Example:
```
{
  "replica_id": "r79e1c033f",
  "persona_id": "p5317866",
  "conversation_name": "My First CVI Conversation"
}
```

Full Request Example with All Options:

{
  "replica_id": "r6ae5b6efc9d", // this is the replica id for the conversation that I created
  "persona_id": "pe9ddc17da43", // this is the persona id for the conversation that I created
  "conversation_name": "Bolt to replace with appropriate name",
  "conversational_context": "Bolt to replace with appropriate context",
  "custom_greeting": "Bolt to replace with appropriate greeting",
  "properties": {
    "max_call_duration": 3600,
    "participant_left_timeout": 60,
    "participant_absent_timeout": 300,
    "language": "english"
  }
}

response example:

```
{
  "conversation_id": "c123456",
  "conversation_name": "A Meeting with Hassaan",
  "status": "active",
  "conversation_url": "https://tavus.daily.co/c123456",
  "replica_id": "r79e1c033f",
  "persona_id": "p5317866",
  "created_at": "2024-08-13T12:34:56Z"
}
```

Step 2: Get the Conversation URL
The API response will include a conversation_url that you can use to join the conversation:

Once you have the conversation_url, you can: Embed the conversation in your website using iframe or Daily SDK:


here is the conversation component:
```
import React, { useEffect, useRef, useState } from 'react';
import DailyIframe from '@daily-co/daily-js';

const getOrCreateCallObject = () => {
  // Use a property on window to store the singleton
  if (!window._dailyCallObject) {
    window._dailyCallObject = DailyIframe.createCallObject();
  }
  return window._dailyCallObject;
};

const TavusConversation = ({ conversationUrl }) => {
  const callRef = useRef(null);
  const [remoteParticipants, setRemoteParticipants] = useState({});

  useEffect(() => {
    // Only create or get one call object per page
    const call = getOrCreateCallObject();
    callRef.current = call;

    // Join meeting
    call.join({ url: conversationUrl });

    // Handle remote participants
    const updateRemoteParticipants = () => {
      const participants = call.participants();
      const remotes = {};
      Object.entries(participants).forEach(([id, p]) => {
        if (id !== 'local') remotes[id] = p;
      });
      setRemoteParticipants(remotes);
    };

    call.on('participant-joined', updateRemoteParticipants);
    call.on('participant-updated', updateRemoteParticipants);
    call.on('participant-left', updateRemoteParticipants);

    // Cleanup
    return () => {
      call.leave();
    };
  }, [conversationUrl]);

  // Attach remote video and audio tracks
  useEffect(() => {
    Object.entries(remoteParticipants).forEach(([id, p]) => {
      // Video
      const videoEl = document.getElementById(`remote-video-${id}`);
      if (videoEl && p.tracks.video && p.tracks.video.state === 'playable' && p.tracks.video.persistentTrack) {
        videoEl.srcObject = new MediaStream([p.tracks.video.persistentTrack]);
      }
      // Audio
      const audioEl = document.getElementById(`remote-audio-${id}`);
      if (audioEl && p.tracks.audio && p.tracks.audio.state === 'playable' && p.tracks.audio.persistentTrack) {
        audioEl.srcObject = new MediaStream([p.tracks.audio.persistentTrack]);
      }
    });
  }, [remoteParticipants]);

  // Custom UI
  return (
    <div className="min-h-screen bg-gray-900 text-white flex flex-col">
      <header className="bg-gray-800 p-4 flex justify-between items-center">
        <span className="font-semibold">Meeting Room (daily-js custom UI)</span>
      </header>
      <main className="flex-1 p-4 grid grid-cols-2 md:grid-cols-4 gap-2">
        {Object.entries(remoteParticipants).map(([id, p]) => (
          <div
            key={id}
            className="relative bg-gray-800 rounded-lg overflow-hidden aspect-video w-48"
          >
            <video
              id={`remote-video-${id}`}
              autoPlay
              playsInline
              className="w-1/3 h-1/3 object-contain mx-auto"
            />
            <audio id={`remote-audio-${id}`} autoPlay playsInline />
            <div className="absolute bottom-2 left-2 bg-black bg-opacity-50 px-2 py-1 rounded text-sm">
              {p.user_name || id.slice(-4)}
            </div>
          </div>
        ))}
      </main>
    </div>
  );
};

export default TavusConversation;

```


When conversation ends, we want to retrieve conversation transcript, create a summary of the conversation with AI, and show it in a new page of the course.
