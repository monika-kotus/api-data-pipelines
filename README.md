# api-data-pipelines
A project focused on building automated data pipelines that collect, process, and store data from APIs. 


# Building a Data Pipeline: Lessons from Code, Clouds, and the Nature of Flow

When I started my recent project, the goal seemed simple: gather data from various APIs, automate the processing, and store it all neatly in a database that refreshes daily. It sounded straightforward, but as I dug in, I realized something unexpected: the structure of my project mirrored the way our minds collect, organize, and connect information. In essence, our minds are like a data pipeline in the cloud — a collection of APIs we’ve accumulated over time, some serving us well and others becoming outdated. This experience of building out my API project brought me some insights about how the mind works, and I’d love to share that journey.

# Step 1: Gathering Data, or “What Am I Even Looking For?”
The first API I tackled was the weather API. This single API brought back a flood of data — temperature, humidity, wind speed, timestamps, and more. The challenge wasn’t just gathering data but figuring out what was useful. Some data keys weren’t always present (like the rain key, which could throw off the entire function if not handled gracefully). Just as we sift through a barrage of information every day, not everything is worth storing.
For this, I had to use methods that could handle missing values gracefully, such as using .get() to retrieve data without crashing the whole process. This is something I learned quickly, especially since weather data can be incomplete or not returned as expected. Here’s a snippet of the code I used for error handling when data was missing:

rain_in_last_3h = item.get("rain", {}).get("3h", 0)  # Safely get rain data

This approach ensured that the program didn’t fail if the key was missing. It’s a bit like the way our minds filter out irrelevant details, focusing instead on key insights while letting the rest fade into the background. We store what matters, or what might serve a future purpose, and leave the rest aside.
After handling one city’s data, I expanded to multiple cities. I used for loops and list comprehensions to efficiently handle tasks iteratively. Here’s the code that looped through each city's data:

weather_items = []
for _, city in cities_df.iterrows():
    # Fetching weather data for each city
    weather_item = {
        "city_id": city_id,
        "forecast_time": item.get("dt_txt"),
        "temperature": item["main"].get("temp"),
        "forecast": item["weather"][0].get("main"),
        "rain_in_last_3h": item.get("rain", {}).get("3h", 0),
        "wind_speed": item["wind"].get("speed"),
        "data_retrieved_at": retrieval_time
    }
    weather_items.append(weather_item)
    
Each time I tackled a new problem, the solution became more reusable, much like how habits and routines form in the mind. Once you find a reliable way to handle a challenge, you can apply it to similar situations in the future.

# Step 2: From Weather to Flights, or “Don’t Get Ahead of Yourself”
Next up was the API for flight data. By this point, I had a bit of confidence — I’d handled weather data, after all! I jumped in quickly, but that confidence quickly turned to frustration. One critical challenge I faced was learning to read and understand API documentation before jumping into the implementation.
I didn’t fully grasp the importance of carefully reviewing the API documentation upfront. It became clear when I started working with the flight API. For example, I overlooked crucial details in the documentation, like understanding which fields were required and how flight data might be missing from certain entries. This became especially evident for fields like arrival_time and flight_number, which weren’t always present for every flight. If I hadn’t spent time reading the documentation thoroughly, I would have been stuck without understanding why some API calls failed.
This experience highlighted the importance of understanding the structure of the data you’re working with before diving in. As I started to properly account for the fields I could expect, I could better handle the inconsistencies. Here’s an example of the approach I used to safely retrieve data, even when some fields weren’t available:

flight_item = {
    "arrival_airport_icao": icao,
    "departure_airport_icao": item["departure"]["airport"].get("icao", None),
    "scheduled_arrival_time": item["arrival"]["scheduledTime"].get("local", None),
    "flight_number": item.get("number", None),
    "data_retrieved_at": retrieval_time
}
The key takeaway here is that reading documentation thoroughly can save you time and effort down the road. It’s like the way we gather information for our mental “databases” — without understanding the sources and structures of the data, it’s easy to build something incomplete or flawed.

# Step 3: Automating, or “Letting Things Flow”
Setting up Cloud Functions was a test of patience. It wasn’t just a matter of uploading the code; I had to configure permissions, adjust environment variables, and, of course, deal with GCP’s tendency to throw cryptic error messages. One particularly frustrating issue involved connecting my function to the Cloud SQL instance. Initially, everything seemed fine — data was being processed locally — but when I tried to push it into my Cloud SQL database, nothing happened.
After some digging, I realized that I had missed a crucial step: I needed to enable the Cloud SQL Admin API and properly configure the Cloud Function’s connection to the Cloud SQL instance. Without this, the function couldn’t communicate with the database, and my data wasn’t flowing.
Once I enabled the Cloud SQL Admin API and completed the necessary configurations, everything clicked into place. This experience served as a reminder that, in both coding and life, it’s the small overlooked steps that often lead to bigger hurdles down the road. Once everything was set up correctly, my Cloud Functions worked seamlessly, and I could automate the process as planned.

# Step 4: Automating and Letting Things Flow
Once I had weather and flight data synchronized and structured, the next step was setting up a daily automation schedule using GCP’s Cloud Scheduler. Watching the data flow smoothly each day, I realized the value of structure and flow. Just as our minds rely on routines — our “mental automations” — we thrive when things run in the background without constant input. Automations, whether in code or in our personal lives, free up energy for the more meaningful work we want to focus on.
I came to think of this project as a kind of mental parallel. Our minds are like databases fed by an endless stream of “APIs” we call life experiences. We encounter new inputs — information, ideas, and lessons — some of which we use daily, others that serve us for a brief period. Over time, we develop systems to organize, retrieve, and even ignore certain types of information.

# Final Reflections: Choosing the Right APIs
Reflecting on this project, I saw my mind’s “data pipeline” with fresh eyes. In life, some inputs — our own “APIs” — are helpful only for a moment. We encounter information, store it, or let it go as it becomes outdated. Others become central to our routines and benefit from a kind of automation. But it’s worth asking: is this “API” serving us, or just cluttering our mental database?
In tech and life, we’re responsible for the APIs we bring into our system and those we choose to let go. Building this project didn’t just teach me about automation — it reminded me that our minds, like data pipelines, thrive on clean, relevant inputs. The art lies in filtering, organizing, and sometimes, just letting go.

