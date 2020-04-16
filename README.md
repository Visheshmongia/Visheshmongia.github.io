# Melody
import os
from tkinter import * 
from pygame import mixer
from tkinter import filedialog
import tkinter.messagebox


#______________________________________________________________________________
#initializing the mixer
mixer.init()  
#______________________________________________________________________________
#defining the window
root = Tk() 

root.geometry('300x300') 
root.title("Melody") 
root.iconbitmap(r'melody.ico')


#______________________________________________________________________________
#Creating the visible text
text =  Label(root,text = 'Lets make some noise!')
text.pack()


#______________________________________________________________________________
#Create the menubar
menubar = Menu(root)
root.config(menu=menubar)


"""Create the submenu"""


#______________________________________________________________________________
#submenu 1
subMenu = Menu(menubar, tearoff=0)
menubar.add_cascade(label="File",menu=subMenu)

def browse_file():
    global filename
    filename = filedialog.askopenfilename()
    print(filename)
subMenu.add_command(label="Open", command=browse_file)
subMenu.add_command(label="Exit", command=root.destroy)


#______________________________________________________________________________
#submenu 2
subMenu = Menu(menubar, tearoff=0)
menubar.add_cascade(label="Help",menu=subMenu)

def about_us():
    tkinter.messagebox.showinfo('Melody','This is a Music Player built using python tkinter by Vishesh Mongia')
subMenu.add_command(label="About Us", command = about_us)


#______________________________________________________________________________
#Creating Play option
def play_music():
    global paused   # Checks if 'paused' is initialized or not
    
    if paused:      # If pause=TRUE
        mixer.music.unpause()
        statusbar['text'] = "Music Resumed"        

        paused = FALSE
    else:           # If pause=FALSE
        try:
            mixer.music.load(filename)
            mixer.music.play()
            statusbar['text']= "Playing Music" + ' - ' +os.path.basename(filename)
        except:
            tkinter.messagebox.showerror('File not found','You have not selected any file to be played')
        
  
        
playPhoto = PhotoImage(file='play.png')
playBtn = Button(root, image=playPhoto, command=play_music)
playBtn.pack()


#______________________________________________________________________________
#Creating Pause option
paused = FALSE

def pause_music():
    global paused
    paused = TRUE
    mixer.music.pause()
    statusbar['text'] = "Music Paused"
    
    
pausePhoto = PhotoImage(file='pause.png')
pauseBtn = Button(root, image=pausePhoto, command=pause_music)
pauseBtn.pack()
 

#______________________________________________________________________________
#Creating Stop option
def stop_music():
    mixer.music.stop()
    statusbar['text'] = "Music Stopped"
    
stopPhoto = PhotoImage(file='stop.png')
stopBtn = Button(root, image=stopPhoto, command=stop_music)
stopBtn.pack()


#______________________________________________________________________________
#Creating Volume option   
def set_vol(val):
    volume = int(val)/100
    mixer.music.set_volume(volume)
    
scale = Scale(root, from_=0, to=100, orient= HORIZONTAL, command=set_vol)
scale.set(70)
mixer.music.set_volume(0.7)
scale.pack() 


#______________________________________________________________________________
#Status Bar
statusbar = Label(root, text="Welcome to Melody", relief=SUNKEN, anchor=W)
statusbar.pack(side=BOTTOM, fill=X)




root.mainloop() 

        
    
    
    
