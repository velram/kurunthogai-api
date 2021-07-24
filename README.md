# kurunthogai-api
Kurunthogai Poems and other relevant information are provided in JSON format

Contents of the JSON API is given
1. Poem index
2. Poem verses
3. Poet's name
4. Poem Thinai type (Type of land where the poem is written)

Sample Kurunthogai JSON object :

{
  "index": 1,
  "poem_verses": "செங்களம் படக் கொன்று அவுணர்த் தேய்த்தசெங் கோல் அம்பின், செங் கோட்டு யானை,கழல் தொடி, சேஎய் குன்றம்குருதிப் பூவின் குலைக் காந்தட்டே.",
  "poet_name": "திப்புத்தோளார்",
  "poem_thinai_type": "குறிஞ்சி"
}

All 401 Kurunthogai poems (JSON objects) are placed in a single JSON array.

Sample JSON array with a single poem

{
  "KurunthogaiPoems": [
    {
      "index": 1,
      "poem_verses": "செங்களம் படக் கொன்று அவுணர்த் தேய்த்தசெங் கோல் அம்பின், செங் கோட்டு யானை,கழல் தொடி, சேஎய் குன்றம்குருதிப் பூவின் குலைக் காந்தட்டே.",
      "poet_name": "திப்புத்தோளார்",
      "poem_thinai_type": "குறிஞ்சி"
    }
  ]
}