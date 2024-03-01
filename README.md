# Anoushka-Yuthika-Arnika
TISB HACKATHON ROUND 1 
#Our code is about aiding people with mental and learning disabilities such as ADHD and Dyslexia, with their education. The application is very simple to use. 
from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.label import Label
from kivy.uix.button import Button
from kivy.uix.popup import Popup
from kivy.uix.textinput import TextInput
from kivy.clock import Clock
from kivy.uix.dropdown import DropDown
from kivy.core.window import Window
import pyttsx3

class MentalHealthApp(App):
    def build(self):
       
        Window.clearcolor = (1, 1, 1, 1)

        self.layout = BoxLayout(orientation='vertical', spacing=20, padding=[20, 40])

        self.title_label = Label(text='AD+ Learn Your Way!', font_name='Arial', font_size=40, color=(0.9, 0.6, 0.4, 1))
        self.layout.add_widget(self.title_label)

        self.language_label = Label(text='Select Language:', font_name='Arial', font_size=20, color=(0.8, 0.5, 0.3, 1))
        self.layout.add_widget(self.language_label)

        self.language_dropdown = DropDown()  
        languages = ['English', 'Spanish', 'French']
        for lang in languages:
            btn = Button(text=lang, size_hint_y=None, height=40, background_color=(0.9, 0.7, 0.5, 1))
            btn.bind(on_release=lambda btn: self.change_language(btn.text.lower()))
            self.language_dropdown.add_widget(btn)

        self.language_button = Button(text='Select', size_hint_y=None, height=50, background_color=(0.8, 0.5, 0.3, 1))
        self.language_button.bind(on_release=self.language_dropdown.open)
        self.layout.add_widget(self.language_button)

        self.label = Label(text='What may we help you with?', font_name='Arial', font_size=20, color=(0.8, 0.5, 0.3, 1))
        self.layout.add_widget(self.label)

        self.button_dyslexia = Button(text='Dyslexia', font_name='Arial', font_size=24, background_color=(0.9, 0.7, 0.5, 1), size_hint_y=None, height=60)
        self.button_dyslexia.bind(on_press=self.choose_dyslexia)
        self.layout.add_widget(self.button_dyslexia)

        self.dyslexia_desc_label = Label(text="Dyslexia is a learning disorder that involves difficulty reading due to problems identifying speech sounds and learning how they relate to letters and words.", font_name='Arial', font_size=14, color=(0.8, 0.5, 0.3, 1))
        self.layout.add_widget(self.dyslexia_desc_label)

        self.button_adhd = Button(text='ADHD', font_name='Arial', font_size=24, background_color=(0.9, 0.7, 0.5, 1), size_hint_y=None, height=60)
        self.button_adhd.bind(on_press=self.choose_adhd)
        self.layout.add_widget(self.button_adhd)

        self.adhd_desc_label = Label(text="ADHD is a neurodevelopmental disorder that affects attention and behavior. Symptoms include difficulty controlling impulsive behaviors and hyperactivity.", font_name='Arial', font_size=14, color=(0.8, 0.5, 0.3, 1))
        self.layout.add_widget(self.adhd_desc_label)

        self.result_label = Label(text='', font_name='Arial', font_size=18, color=(0.8, 0.5, 0.3, 1))
        self.layout.add_widget(self.result_label)

        self.text_input = TextInput(hint_text='Enter your study material here', multiline=True, font_size=18, size_hint_y=None, height=150)
        self.read_button = Button(text="Read Aloud", background_color=(0.8, 0.5, 0.3, 1), size_hint_y=None, height=50)
        self.read_button.bind(on_press=self.read_aloud)
        self.increase_font_button = Button(text="Increase Font Size", font_name='Arial', font_size=20, on_press=self.increase_font_size, background_color=(0.9, 0.6, 0.4, 1), size_hint_y=None, height=50)
        self.decrease_font_button = Button(text="Decrease Font Size", font_name='Arial', font_size=20, on_press=self.decrease_font_size, background_color=(0.9, 0.6, 0.4, 1), size_hint_y=None, height=50)

        self.engine = pyttsx3.init()
        self.popup = None

        return self.layout

    def change_language(self, lang_code):
        translations = {
            'english': {
                'title': 'AD+ Learn Your Way!',
                'language_label': 'Select Language:',
                'button_dyslexia': 'Dyslexia',
                'button_adhd': 'ADHD',
                'label': 'What may we help you with?',
                'read_button': 'Read Aloud',
                'increase_font_button': 'Increase Font Size',
                'decrease_font_button': 'Decrease Font Size',
                'popup_content': 'Stay focused and productive!',
                'dyslexia_desc': "Dyslexia is a learning disorder that involves difficulty reading due to problems identifying speech sounds and learning how they relate to letters and words.",
                'adhd_desc': "ADHD is a neurodevelopmental disorder that affects attention and behavior. Symptoms include difficulty controlling impulsive behaviors and hyperactivity."
            },
            'spanish': {
                'title': 'AD+ Aprende a tu manera!',
                'language_label': 'Seleccionar Idioma:',
                'button_dyslexia': 'Dislexia',
                'button_adhd': 'TDAH',
                'label': '¿Cómo podemos ayudarte?',
                'read_button': 'Leer en voz alta',
                'increase_font_button': 'Aumentar tamaño de fuente',
                'decrease_font_button': 'Reducir tamaño de fuente',
                'popup_content': '¡Mantente enfocado y productivo!',
                'dyslexia_desc': "La dislexia es un trastorno del aprendizaje que implica dificultades para leer debido a problemas para identificar los sonidos del habla y aprender cómo se relacionan con las letras y las palabras.",
                'adhd_desc': "TDAH es un trastorno del neurodesarrollo que afecta la atención y el comportamiento. Los síntomas incluyen dificultad para controlar los comportamientos impulsivos e hiperactividad."
            },
            'french': {
                'title': 'AD+ Apprenez à votre façon!',
                'language_label': 'Sélectionnez la langue:',
                'button_dyslexia': 'Dyslexie',
                'button_adhd': 'TDAH',
                'label': 'Comment pouvons-nous vous aider?',
                'read_button': 'Lire à haute voix',
                'increase_font_button': 'Augmenter la taille de la police',
                'decrease_font_button': 'Diminuer la taille de la police',
                'popup_content': 'Restez concentré et productif!',
                'dyslexia_desc': "La dyslexie est un trouble de l'apprentissage qui implique des difficultés à lire en raison de problèmes pour identifier les sons de la parole et apprendre comment ils se rapportent aux lettres et aux mots.",
                'adhd_desc': "TDAH est un trouble du neurodéveloppement qui affecte l'attention et le comportement. Les symptômes incluent des difficultés à contrôler les comportements impulsifs et l'hyperactivité."
            }
        }

        translations = translations.get(lang_code, translations['english'])  

        self.title_label.text = translations['title']
        self.language_label.text = translations['language_label']
        self.button_dyslexia.text = translations['button_dyslexia']
        self.button_adhd.text = translations['button_adhd']
        self.label.text = translations['label']
        self.read_button.text = translations['read_button']
        self.increase_font_button.text = translations['increase_font_button']
        self.decrease_font_button.text = translations['decrease_font_button']
        self.popup_content = translations['popup_content']
        self.dyslexia_desc_label.text = translations['dyslexia_desc']
        self.adhd_desc_label.text = translations['adhd_desc']

    def choose_dyslexia(self, instance):
        self.layout.clear_widgets()
        self.label.text = "Please enter your study material:"
        self.layout.add_widget(self.label)
        self.layout.add_widget(self.text_input)
        self.layout.add_widget(self.read_button)
        self.layout.add_widget(self.increase_font_button)
        self.layout.add_widget(self.decrease_font_button)

    def choose_adhd(self, instance):
        self.layout.clear_widgets()
        self.label.text = "Don't Give up & Don't get distracted. Enter your study material here to help you get through it."
        self.layout.add_widget(self.label)
        self.layout.add_widget(self.text_input)
        self.layout.add_widget(self.read_button)
        self.layout.add_widget(self.result_label)

        self.popup_event = Clock.schedule_interval(self.show_popup, 1 * 3)

    def show_popup(self, dt):
        if self.popup is None:
            self.popup = Popup(title='Gentle Reminder',
                               content=Label(text='Stay focused and productive!', font_name='Arial', font_size=20, color=(0.2, 0.3, 0.8, 1)),
                               size_hint=(None, None), size=(400, 200))
            self.popup.open()

    def read_aloud(self, instance):  
        text_to_read = self.text_input.text
        if text_to_read:
            self.engine.say(text_to_read)
            self.engine.runAndWait()
        else:
            self.result_label.text = "Please enter text before trying to read aloud."

    def increase_font_size(self, instance):
        current_font_size = self.text_input.font_size
        if current_font_size < 50:
            self.text_input.font_size += 2

    def decrease_font_size(self, instance):
        current_font_size = self.text_input.font_size
        if current_font_size > 10:
            self.text_input.font_size -= 2

if _name_ == '_main_':
    MentalHealthApp().run()
