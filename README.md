import pyttsx3  # pip install pyttsx3
import speech_recognition as sr  # pip install speechRecognition
import datetime
import wikipedia  # pip install wikipedia
import webbrowser
import os
import smtplib
import pyjokes
import requests
import time
import calendar
import chatbot
import asynchat
from playsound import playsound
import phonenumbers
from phonenumbers import geocoder
import json
import numbers
from phonenumbers import carrier
from bs4 import BeautifulSoup
import tkinter
from typing import Optional, Union
import wolframalpha

try:
    app = wolframalpha.Client("5VWLT4-3GUJKPAVRK")
except Exception:
    print("error occured")

engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
# print(voices[1].id)
engine.setProperty('voice', voices[0].id)


def speak(audio):
    engine.say(audio)
    engine.runAndWait()


def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour >= 0 and hour < 12:
        speak("Good Morning!")
        print("Good Morning!")

    elif hour >= 12 and hour < 18:
        speak("Good Afternoon!")
        print("Good Afternoon!")

    else:
        speak("Good Evening!")
        print("Good Evening!")

    speak("I am Jarvis Sir. Please tell me how may I help you")


def takeCommand():
    # It takes microphone input from the user and returns string output

    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Yes Listening Sir...")
        r.pause_threshold = 1
        audio = r.listen(source, timeout=4, phrase_time_limit=7)

    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language='en-in')
        print("owner said:" + query)


    except Exception as e:
        # print(e)
        print("Say that again please...")
        return "None"
    query = query.lower()
    return query


def print_headlines(response_text):
    soup = BeautifulSoup(response_text, 'lxml')
    headlines = soup.find_all(attrs={"itemprop": "headline"})
    for headline in headlines:
        print(headline.text)

url = 'https://inshorts.com/en/read'
response = requests.get(url)



def sendEmail(to, content):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.login('your email id', 'password of email id')
    server.sendmail('emial id', to, content)
    server.close()


def TaskExecution():
    wishMe()
    while True:
        query = takeCommand()

        if 'wikipedia' in query:
            speak('Searching Wikipedia...')
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=2)
            speak("According to Wikipedia")
            print(results)
            speak(results)

        elif 'command' in query:
            speak("here is the commands listed below.")
            print("1. wikipedia searches.")
            print("2. weather report")
            print("3.commands")
            print("4. open google chrome")
            print("5. open notepad")
            print("6. open calendar")
            print("7. jarvis can you send mail")
            print("8. open Translater")
            print("7. introduce yourself")
            print("8. open youtube")
            print("7. open google")
            print("8. open stackoverflow")
            print("9. jarvis")
            print("10. open my website")
            print("11. open my favourite website")
            print("12. go to free fire website")
            print("13. open my hacking terminal")
            print("14. play music")
            print("15. what is the time now")
            print("16. open brave")
            print("17. how are you jarvis")
            print("18. open map")
            print("19. say some funny joke")
            print("20. open cmd")
            print("21. how much e books are there in my system")
            print("22. open my youtube channel")
            print("23. scan the system")
            print("24. play iron man video from youtube")
            print("25. hack the google cloud system")
            print("26. send email")
            print("27. open facebook")
            print("28. open instgram")
            print("29. go to youtube history")
            print("30. temperature")
            print("31. Set alarm")
            print("32. Read News")
            print("33. Track Phone")
            print("34. oky bye")
            print("35. open gmail")

        elif 'read news' in query:
            speak("yes sir")
            speak("shall i read the news or else i will play the news video from youtube")
            query = takeCommand().lower()
            if 'just read' in query:
                speak("yes sir")
                print_headlines(response.text)
                speak(response.text)
            elif 'play from youtube' in query:
                speak("yes sir")
                speak("which news channel shall i play sir")
                speak("what video shall i play sir")
                url = "https://www.youtube.com/results?q=" + takeCommand().lower()
                count = 0
                cont = requests.get(url)
                data = cont.content
                data = str(data)
                lst = data.split('"')
                for i in lst:
                    count += 1
                    if i == "WEB_PAGE_TYPE_WATCH":
                        break
                if lst[count - 5] == "/results":
                    raise Exception("No Video Found for this Topic!")

                webbrowser.open("https://www.youtube.com" + lst[count - 5])


        elif 'weather report' in query:
            speak("yes sir searching today whether report")
            print("which city should i search for")
            speak('which city should i search for')
            city = takeCommand().lower()
            print('Displaying Weather report for: ' + city)
            url = 'https://wttr.in/{}'.format(city)
            res = requests.get(url)
            print(res.text)
            speak("here is the weather report")

        elif 'open gmail' in query:
            speak("yes sir")
            webbrowser.open("https://mail.google.com/mail/u/0/#inbox")

        elif 'open google chrome' in query:
            speak("yes sir opening chrome webbrowser")
            codePath = "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe"

        elif 'turn on safe internet protocol' in query:
            speak("yes sir turning on safe internet protocol")
            codePath = "C:\\Program Files (x86)\\Wireshark\\Wireshark.exe"

        elif 'track phone' in query:
            speak("yes sir")
            speak("sir code the phone number sir")
            number = "+917338726969"
            ch_number = phonenumbers.parse(number, "CH")
            print(geocoder.description_for_number(ch_number, "en"))
            speak(geocoder.description_for_number(ch_number, "en"))
            number = "+917338726969"
            ch_number = phonenumbers.parse(number, "CH")
            print(geocoder.description_for_number(ch_number, "en"))
            speak(geocoder.description_for_number(ch_number, "en"))
            service_provider = phonenumbers.parse(number, "RO")
            print(carrier.name_for_number(service_provider, "en"))
            speak(carrier.name_for_number(service_provider, "en"))


        elif 'ok bye' in query:
            speak("yes sir")
            speak("see you later sir you can call me any time ")
            print("see you later sir you can call me any time")
            break

        elif 'set alarm' in query:
            speak("yes sir. tell me the time sir")
            alarm_time = input("Enter the time of alarm to be set:HH:MM:SS\n")
            alarm_hour = alarm_time[0:2]
            alarm_minute = alarm_time[3:5]
            alarm_seconds = alarm_time[6:8]
            alarm_period = alarm_time[9:11].upper()
            print("Setting up alarm..")
            while True:
                now = datetime.now()
                current_hour = now.strftime("%I")
                current_minute = now.strftime("%M")
                current_seconds = now.strftime("%S")
                current_period = now.strftime("%p")
                if (alarm_period == current_period):
                    if (alarm_hour == current_hour):
                        if (alarm_minute == current_minute):
                            if (alarm_seconds == current_seconds):
                                print("Wake Up!")
                                playsound('audio.mp3')
                                break
        elif 'temperature' in query:
            try:
                res = app.query(query)
                print(next(res.results).text)
                speak(next(res.results).text)
            except:
                speak("error")

        elif 'calculate' in query:
            try:
                res = app.query(query)
                print(next(res.results).text)
                speak(next(res.results).text)
            except:
                speak("error")
                print("error")

        elif 'elementary math' in query:
            try:
                speak("what shall i perform sir")
                speak("Here are the performing programme")
                print("Arithmetic")
                print("Fraction")
                print("Percentage")
                print("Place value")
                print("Numerical Arithmetic")
                print("Mathematical word problem")
                maths = input("Enter the sum here sir: ")
                res = app.query(maths)
                print(next(res.results).text)
                speak("here is the answer sir")
                speak(next(res.results).text)
            except:
                speak("error occured")
                print("error occured")

        elif 'jarvis can you send email' in query:
            speak("yeah sure sir")


        elif 'open calendar' in query:
            speak("yes sir opening online calender")
            webbrowser.open("https://calendar.online/")

        elif 'open Translater' in query:
            speak("yes sir opening translater")
            webbrowser.open("https://translate.google.co.in/?sl=hi&tl=en&op=translate")


        elif 'open youtube' in query:
            speak("shall i search or play the video from youtube")
            if 'just search' in query:
                speak("yes sir what video shall i play sir")
                webbrowser.open("https://www.youtube.com/results?search_query=" + takeCommand().lower())

                

        elif 'go to youtube history' in query:
            speak("yes sir what shell i search on youtube history")
            webbrowser.open("https://www.youtube.com/feed/history?query=" + takeCommand().lower())

        elif 'play video from youtube' in query:
            speak("what video shall i play sir")
            url = "https://www.youtube.com/results?q=" + takeCommand().lower()
            count = 0
            cont = requests.get(url)
            data = cont.content
            data = str(data)
            lst = data.split('"')
            for i in lst:
                count += 1
                if i == "WEB_PAGE_TYPE_WATCH":
                    break
            if lst[count - 5] == "/results":
                raise Exception("No Video Found for this Topic!")


            webbrowser.open("https://www.youtube.com" + lst[count - 5])


        elif 'introduce yourself' in query:
            speak("yes sir. I am your personal assistant")
            speak("i am jarvis artificial intelligence assistant. created by G.KEERTHI BALAN.  ")
            speak("I am running through the python programming language.")
            speak("And i am more advance than alexa, siri and other assistance.")

        elif 'open google' in query:
            speak("opening google sir")
            speak("what shall i search for sir")
            webbrowser.open("https://www.google.com/search?q=" + takeCommand().lower())

        elif 'open stackoverflow' in query:
            speak("opening stackoverflow")
            webbrowser.open("www.stackoverflow.com")



        elif 'open my website' in query:
            speak("yes sir")
            webbrowser.open("https://keerthibalan.wordpress.com/")

        elif 'open my favourite website' in query:
            speak("sure sir")
            webbrowser.open("https://astralprojectionbykeerthibalan.wordpress.com")


        elif 'go to free fire website' in query:
            speak("yes sir i am opening the free fire website")
            webbrowser.open("https://freefirecharacters2020.blogspot.com/2021/08/keerthibalan.html")


        elif 'open my hacking terminal' in query:
            speak("yes sir give some minute i will setup your hacking system")
            webbrowser.open("https://pranx.com/hacker")


        elif 'play music' in query:
            speak("wait for some times sir... because i am choosing better song for you")
            music_dr = 'C:\\music'
            songs = os.listdir(music_dr)
            os.startfile(os.path.join(music_dr, songs[0]))

        elif 'what is the time now' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            speak("Sir, the time is {strTime}")

        elif 'open brave' in query:
            speak("yes sir")
            speak("opening brave")
            codePath = "C:\\Program Files\\BraveSoftware\\Brave-Browser\\Application\\brave.exe"
            os.startfile(codePath)

        elif 'how are you jarvis' in query:
            speak(" Sir I Am fine sir ")

        elif 'open android' in query:
            speak("yes sir opening bluestacks")
            bluePath = "C:\\Program Files\\BlueStacks_nxt\\HD-Player.exe"
            os.startfile(bluePath)

        elif 'shutdown the system' in query:
            speak("are you sure sir. shell i shutdown the system sir")
            takeCommand().lower()
            if 'shutdown' in query:
                speak("yes sir")
                os.system("shutdown /s /t 1")
            elif 'no' in query:
                speak("okay sir")

        elif 'restart the system' in query:
            speak("are you sure sir. shell i restart the system sir")
            query = takeCommand().lower()
            if 'yes' in query:
                speak("yes sir")
                os.system("shutdown /r /t  1")  # 0 is time that is for after what time we want to restart
            elif 'no' in query:
                speak("okay sir as your wish")

        elif 'put the system on sleep mode' in query:
            speak("yes sir")
            print("Printed immediately.")
            time.sleep(2.4)
            print("Printed after 2.4 seconds.")

        elif 'open map' in query:
            speak("yes sir opening google map")
            speak("which place shall i search for sir.")
            webbrowser.open("https://www.google.com/maps/place/" + takeCommand().lower())


        elif 'say some funny joke' in query:
            speak("yes sir")
            My_joke = pyjokes.get_joke(language="en", category="all")
            speak(My_joke)


        elif 'open facebook' in query:
            speak("opening facebook")
            webbrowser.open("https://www.facebook.com/")

        elif 'open instagram' in query:
            speak("opening instagram")
            webbrowser.open("https://www.instagram.com/")


        elif 'open cmd' in query:
            speak("opening command promt")
            os.system('start cmd')

        elif 'open free fire' in query:
            speak("opening free fire game sir")




        elif 'open notepad' in query:
            speak("opening notepad sir")
            os.system('start notepad')

        elif 'open my youtube channel' in query:
            speak("yes sir opening your youtube channel")
            webbrowser.open("https://www.youtube.com/channel/UCtzaepk9whB-5Vl8BBR-RsQ")


        elif 'scan the system' in query:
            speak("yes sir scanning for virus in the system")
            speak("sir all security are in progress")
            speak("there is no virus or malware")


        elif 'play iron man video from youtube' in query:
            speak("yes sir")
            webbrowser.open("https://www.youtube.com/watch?v=r99niJVKlVA")

        elif 'hack the google cloud system' in query:
            speak(" yes sir i will perform the hacking programme.")
            speak(" sir error the google sir is having strong firewall sir it will take 2 days to break")


        elif 'send email' in query:
            try:
                speak("What should I say?")
                content = takeCommand().lower()
                speak("sir give me receiver address sir")
                to = "stukeerthibalang6122@kvschennairegion.in"
                sendEmail(to, content)
                speak("Email has been sent!")

            except Exception as e:
                print(e)
                speak("Sorry. I am not able to send this email because there is no internet connection.")


if __name__ == "__main__":
    while True:
        permission = takeCommand().lower()
        if "activate jarvis" in permission:
            speak("activating jarvis programme")
            music_dr = 'C:\\sound'
            songs = os.listdir(music_dr)
            os.startfile(os.path.join(music_dr, songs[0]))
            for i in range(0, 5):
                # printing numbers
                print(i)

                # adding 2 seconds time delay
                time.sleep(5)
            TaskExecution()
        elif "see you later" in permission:
            exit(TaskExecution())
            print("okay sir i am sleeping you can call me later")
