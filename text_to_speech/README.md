# Convert any .pdf file 📚 into an audio 🔈 book with Python

A while ago I was messing around with google's Text to Speech python library.
This library basically reads out any piece of text and converts it to `.mp3` file. Then I started thinking of making something useful out of it.

### My installed, saved, and unread pdf books 😕
I like reading books. I really do. I think language and ideas sharing is fascinating. I have a directory at which I store pdf books that I plan on reading but I never do. So I thought hey, why dont I make them audio books and listen to them while I do something else 😄!

So I started planning how the script should look like.

 - Allow user to pick a `.pdf file`
 - Convert the file into one string
 - Output `.mp3` file.

Without further needless words, lets get to it.

### Allow user to pick a .pdf file
Python can read files easily. I just need to use the method `open("filelocation", "rb")` to open the file in reading mode. I dont want to be copying and pasting files to the directory of the code everytime I want to use the code though. So to make it easier we will use `tkinter library` to open up an interface that lets us choose the file.

```python
from tkinter import Tk
from tkinter.filedialog import askopenfilename

Tk().withdraw() # we don't want a full GUI, so keep the root window from appearing
filelocation = askopenfilename() # open the dialog GUI
```

Great. Now we have the file location stored in a `filelocation` variable.
> Allow user to pick a .pdf file ✔️

### Convert the file into one string
As I said before, to open a file in Python we just need to use the `open()` method. But we also want to convert the pdf file into regular pieces of text. So we might as well do it now.
To do that we will use a library called `pdftotext`.
Lets install it:
```
sudo pip install pdftotext
```

Then:
```python
from tkinter import Tk
from tkinter.filedialog import askopenfilename
import pdftotext

Tk().withdraw() # we don't want a full GUI, so keep the root window from appearing
filelocation = askopenfilename() # open the dialog GUI

with open(filelocation, "rb") as f:  # open the file in reading (rb) mode and call it f
    pdf = pdftotext.PDF(f)  # store a text version of the pdf file f in pdf variable
```
Great. Now we have the file stored in the variable `pdf`.
if you print this variable, you will get an array of strings. Each string is a line in the file. to get them all into one `.mp3` file, we will have to make sure they are all stored as one string. So lets loop through this array and add them all to one string.

```python
from tkinter import Tk
from tkinter.filedialog import askopenfilename
import pdftotext

Tk().withdraw() # we don't want a full GUI, so keep the root window from appearing
filelocation = askopenfilename() # open the dialog GUI

with open(filelocation, "rb") as f:  # open the file in reading (rb) mode and call it f
    pdf = pdftotext.PDF(f)  # store a text version of the pdf file f in pdf variable

string_of_text = ''
for text in pdf:
    string_of_text += text
```

Sweet 😄. Now we have it all as one piece of string.
> Convert the file into one string ✔️

### Output .mp3 file 🔈
Now we are ready to use the `gTTS` (google Text To Speech) library. all we need to do is pass the string we made, store the output in a variable, then use the `save()` method to output the file to the computer.
Lets install it:

```
sudo pip install gtts
```

Then:
```python
from tkinter import Tk
from tkinter.filedialog import askopenfilename
import pdftotext
from gtts import gTTS

Tk().withdraw() # we don't want a full GUI, so keep the root window from appearing
filelocation = askopenfilename() # open the dialog GUI

with open(filelocation, "rb") as f:  # open the file in reading (rb) mode and call it f
    pdf = pdftotext.PDF(f)  # store a text version of the pdf file f in pdf variable

string_of_text = ''
for text in pdf:
    string_of_text += text

final_file = gTTS(text=string_of_text, lang='en')  # store file in variable
final_file.save("Generated Speech.mp3")  # save file to computer
```