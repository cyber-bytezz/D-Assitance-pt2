
# An Interactive Virtual Assistant

A versatile virtual assistant designed to interact with users through voice commands. This project leverages a variety of libraries and APIs to provide a wide range of functionalities, from basic system interactions to advanced computational intelligence.

## Table of Contents
- [Module Imports](#module-imports)
- [Memory and Constants](#memory-and-constants)
- [Function Definitions](#function-definitions)
- [Startup and Wish Functions](#startup-and-wish-functions)
- [MainThread Class](#mainthread-class)
- [Functionalities](#functionalities)
- [GUI Initialization and Startup](#gui-initialization-and-startup)
- [Application Execution](#application-execution)
- [Event Handling](#event-handling)
- [Acknowledgements](#acknowledgements)

---

## Module Imports

In this project, we've imported various libraries to enable different functionalities:

- `JarvisAssistant` module is the core component of our virtual assistant.
- `re` is used for regular expressions, which allows us to process and recognize patterns in user commands.
- `os` provides functions for interacting with the operating system, such as launching applications and managing files.
- `random` helps in generating random responses for greetings.
- `pprint` is used for pretty-printing data structures, making it easier to read and understand complex outputs.
- `datetime` provides functions to work with dates and times, allowing us to provide current time and date information.
- `requests` enables us to make HTTP requests, which is useful for tasks like fetching weather information.
- `sys` provides access to system-specific parameters and functions, allowing us to interact with the system.
- `urllib.parse` is used for parsing URLs, which can be handy for web-related tasks.
- `pyjokes` allows us to generate jokes, adding a touch of humor to our virtual assistant.
- `time` provides various time-related functions, which can be used for adding delays or controlling time-sensitive tasks.
- `pyautogui` gives us control over the mouse and keyboard, enabling automation of various tasks.
- `pywhatkit` is a library for automating tasks related to web services like WhatsApp and YouTube.
- `wolframalpha` allows us to interact with the Wolfram Alpha API for computational intelligence tasks.
- `Image` from the `PIL` library is used for handling images, which is important for tasks like taking screenshots.

## Memory and Constants

We've defined several lists and dictionaries to help our assistant understand and respond to user inputs:

- `GREETINGS` contains various greetings that users might use to initiate interactions with our virtual assistant.
- `GREETINGS_RES` provides corresponding responses to those greetings, making the interaction more natural and engaging.
- `EMAIL_DIC` is a dictionary mapping user-defined email aliases to actual email addresses, facilitating email sending functionality.
- `CALENDAR_STRS` consists of phrases that users might use to inquire about their schedule.

## Function Definitions

### `speak(text)`

This function plays a crucial role in the interaction with the user. It converts text into speech, allowing the assistant to communicate effectively.

### `computational_intelligence(question)`

Here, we interact with the Wolfram Alpha API, sending a user's question and receiving an intelligent response. This adds a layer of computational intelligence to our assistant's capabilities.

## Startup and Wish Functions

- `startup()`: This function handles the initialization process when the assistant is first launched. It checks and calibrates various system components, ensuring everything is in order for a smooth interaction.
- `wish()`: Depending on the time of day, this function greets the user appropriately, setting a friendly tone for the interaction.

## MainThread Class

The `MainThread` class is pivotal in handling the execution of tasks. It contains the `TaskExecution` method, which is responsible for managing various functionalities based on user input.

## Functionalities

### Getting Date and Time

The assistant provides the current date and time upon user request.

```python
if re.search('date', command):
    date = obj.tell_me_date()
    speak(date)
elif "time" in command:
    time_c = obj.tell_time()
    speak(f"Sir the time is {time_c}")
```

### Application Launching

Users can instruct the assistant to launch specific applications, like Google Chrome.

```python
elif re.search('launch', command):
    dict_app = {
        'chrome': 'C:/Program Files/Google/Chrome/Application/chrome'
    }

    app = command.split(' ', 1)[1]
    path = dict_app.get(app)

    if path is None:
        speak('Application path not found')
    else:
        speak('Launching: ' + app + ' for you sir!')
        obj.launch_any_app(path_of_app=path)
```

### Opening Websites

Users can ask the assistant to open specific websites.

```python
elif re.search('open', command):
    domain = command.split(' ')[-1]
    open_result = obj.website_opener(domain)
    speak(f'Alright sir !! Opening {domain}')
```

### Checking Weather

The assistant fetches and communicates the current weather in a designated city.

```python
elif re.search('weather', command):
    city = command.split(' ')[-1]
    weather_res = obj.weather(city=city)
    speak(weather_res)
```

### Information Retrieval

Users can request information about various topics, which the assistant fetches and communicates.

```python
elif re.search('tell me about', command):
    topic = command.split(' ')[-1]
    if topic:
        wiki_res = obj.tell_me(topic)
        speak(wiki_res)
    else:
        speak("Sorry sir. I couldn't load your query from my database. Please try again")
```

### Reading News

The assistant provides top headlines from The Times Of India.

```python
elif "buzzing" in command or "news" in command or "headlines" in command:
    news_res = obj.news()
    speak('Source: The Times Of India')
    speak('Todays Headlines are..')
    for index, articles in enumerate(news_res):
        pprint.pprint(articles['title'])
        speak(articles['title'])
        if index == len(news_res)-2:
            break
    speak('These were the top headlines, Have a nice day Sir!!..')
```

### Searching on Google

Users can ask the assistant to perform a Google search.

```python
elif 'search google for' in command:
    obj.search_anything_google(command)
```

### Playing Music

The assistant can play music from a predefined directory.

```python
elif "play music" in command or "hit some music" in command:
    music_dir = "F://Songs//Imagine_Dragons"
    songs = os.listdir(music_dir)
    for song in songs:
        os.startfile(os.path.join(music_dir, song))
```

### Playing YouTube Videos

Users can request the assistant to play specific videos on YouTube.

```python
elif 'youtube' in command:
    video = command.split(' ')[1]
    speak(f"Okay sir, playing {video} on youtube")
    pywhatkit.playonyt(video)
```

### Sending Emails

The assistant facilitates email composition and sending

 based on user instructions.

```python
elif "email" in command or "send email" in command:
    # Email sending functionality
```

### Computational Intelligence

Users can ask the assistant to perform calculations or provide factual information, which is powered by the Wolfram Alpha API.

```python
elif "calculate" in command or "what is" in command:
    question = command
    answer = computational_intelligence(question)
    speak(answer)
```

### Location Services

The assistant can provide information about the location of a specified place.

```python
elif "where is" in command:
    place = command.split('where is ', 1)[1]
    current_loc, target_loc, distance = obj.location(place)
    # Location information is communicated to the user
```

### Getting IP Address

The assistant communicates the user's public IP address.

```python
elif "ip address" in command:
    ip = requests.get('https://api.ipify.org').text
    speak(f"Your ip address is {ip}")
```

### Switching Windows

Users can instruct the assistant to switch between open windows.

```python
elif "switch the window" in command or "switch window" in command:
    pyautogui.keyDown("alt")
    pyautogui.press("tab")
    time.sleep(1)
    pyautogui.keyUp("alt")
```

### Finding Current Location

The assistant provides information about the user's current location.

```python
elif "where i am" in command or "current location" in command or "where am i" in command:
    city, state, country = obj.my_location()
    speak(f"You are currently in {city} city which is in {state} state and country {country}")
```

### Taking Screenshots

Users can instruct the assistant to take a screenshot, which is then saved with a specified name.

```python
elif "take screenshot" in command or "take a screenshot" in command or "capture the screen" in command:
    speak("By what name do you want to save the screenshot?")
    name = obj.mic_input()
    speak("Alright sir, taking the screenshot")
    img = pyautogui.screenshot()
    name = f"{name}.png"
    img.save(name)
    speak("The screenshot has been succesfully captured")
```

### Displaying Screenshots

This functionality allows the user to view the captured screenshot. The assistant attempts to open and display the image, providing feedback on success or failure.

```python
elif "show me the screenshot" in command:
    try:
        img = Image.open('D://JARVIS//JARVIS_2.0//' + name)
        img.show(img)
        speak("Here it is sir")
        time.sleep(2)
    except IOError:
        speak("Sorry sir, I am unable to display the screenshot")
```

### Hiding/Showing Files

The assistant can hide all files in a specified directory, enhancing privacy.

```python
elif "hide all files" in command or "hide this folder" in command:
    os.system("attrib +h /s /d")
    speak("Sir, all the files in this folder are now hidden")
```

This functionality reverses the hiding operation, making all files in the directory visible.

```python
elif "visible" in command or "make files visible" in command:
    os.system("attrib -h /s /d")
    speak("Sir, all the files in this folder are now visible to everyone. I hope you are taking this decision in your own peace")
```

### Exiting Jarvis

The user can instruct the assistant to go offline and terminate the program.

```python
elif "goodbye" in command or "offline" in command or "bye" in command:
    speak("Alright sir, going offline. It was nice working with you")
    sys.exit()
```

---

## GUI Initialization and Startup

### `MainThread` Class

The `MainThread` class is the heart of task execution. It handles the initialization, greeting, and execution of user commands.

### GUI Initialization

PyQt5 is used to create a graphical user interface. This includes loading images (such as live wallpaper and initiation animation) and setting up timers for real-time updates.

### Application Startup

The GUI starts with an engaging live wallpaper and initiation animation, providing a visually appealing user experience.

### Application Execution

The main thread is launched, which handles user commands. Additionally, a timer is started for real-time updates in the GUI.

### Event Handling

Button clicks within the GUI are handled. For instance, the "Start" button triggers tasks, while the "Close" button exits the application.

---

## Acknowledgements

This project is made possible by the collective effort of the development team. Special thanks to the creators of the libraries and APIs used.

---

Your code is comprehensive, covering a wide range of functionalities, from basic tasks like providing the time to more complex operations like sending emails and interacting with web services. The graphical user interface enhances user experience, making interactions intuitive.

If you have any specific questions or if there's a particular aspect you'd like to discuss in more detail, feel free to let me know!
```

This README file provides an in-depth explanation of your project, including module imports, memory and constants, function definitions, startup procedures, main thread functionality, a detailed breakdown of each functionality, GUI initialization and event handling,
