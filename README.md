# Minerva_lite
import playsound
from gtts import gTTS
import random
import speech_recognition as sr
import datetime

def listen():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        #print('Скажите команду: ')
        audio = r.listen(source)

    try:
        speech=r.recognize_google(audio, language='ru')
        return(speech)
    except sr.UnknownValueError:
        return('Error')
    except sr.RequestError as e:
        return('Error')


def say(text):
    voice=gTTS(text, lang='ru')
    unique_filename='audio_'+str(random.randint(0,100000))+'.mp3'
    voice.save(unique_filename)
    playsound.playsound(unique_filename)
    #print("Минерва :",text)

def handle_message(message):
    if 'привет' in message.lower():
        say('Привет')
    if 'прощай' in message.lower():
        finish()
    if 'минерва' in message.lower():
        say('Да Никита, я тебя слушаю')
    if 'который час' in message.lower() or 'сколько сейчас времени' in message.lower():
        now_date = str(datetime.datetime.now())
        minutes=int(now_date[14:16])
        hours=int(now_date[11:13])
        full_answer='Сейчас '+ str(hours) +' час '+str(minutes)+' минут'
        say(full_answer)
    if 'какой сегодня день' in message.lower():
        now_date = str(datetime.datetime.now())
        year_now = int(now_date[0:4])
        month_now = int(now_date[5:7])
        day_now=int(now_date[8:10])
        if month_now==1:
            month='Января'
        elif month_now==2:
            month='февраля'
        elif month_now==3:
            month='марта'
        elif month_now==4:
            month='апреля'
        elif month_now==5:
            month='майя'
        elif month_now==6:
            month='июня'
        elif month_now==7:
            month='июля'
        elif month_now==8:
            month='августа'
        elif month_now==9:
            month='сентября'
        elif month_now==10:
            month='октября'
        elif month_now==11:
            month='ноября'
        elif month_now==12:
            month='декабря'
        full_answer='Сегодня '+str(day_now)+' '+str(month)+' '+str(year_now)+' года!'
        say(full_answer)
    else:
        say('Я такой команды не знаю!')

def finish():
    say('До встречи!')
    exit()

if __name__=='__main__':
    #print('Test')

    while True:
        command=listen()
        handle_message(command)
