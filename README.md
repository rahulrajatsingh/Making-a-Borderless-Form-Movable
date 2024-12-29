Introduction
This article defines a way to make a borderless form movable. There are many scenarios where we may want to remove the default windows border but still want the form to be movable. This article will explore one way of doing the same.

Background
I read a similar article few days back here. I tried to do this myself long back. This article presents a very good way of moving the form but the logic for movement sometimes does not result in a smooth movement and sometimes the form flickers a lot while moving. So I put my preferred way of making a border less form movable and someone suggested I should have it as an alternative to this. Here, I am presenting the alternative way of doing the same thing in this article.

Using the Code
The first thing that we need to do is to make the form borderless. This can easily be done by setting the FormBorderStyle property of this form to None.

Image 1

After we set this property to null, the Form will look like:

Image 2

The next step would be to handle the MouseDown event for the form. This event will contain the code that will perform the form movement operation when the mouse is pressed and dragged.

Image 3

The basic idea behind this technique is to use the windows messages to handle the form movement. What we will do is whenever the user presses the mouse button and moves the mouse, we will send a message to move the form according to the mouse movement. For this, we first need to have the possibility of sending the windows messages. First, we will have to use the interop services by using the namespace as:

C#
using System.Runtime.InteropServices;
The next thing would be to define the messages that will take care of moving the form. We will have these as class member variables:

C#
public const int WM_NCLBUTTONDOWN = 0xA1;
public const int HT_CAPTION = 0x2;
[DllImportAttribute("user32.dll")]
public static extern int SendMessage(IntPtr hWnd, int Msg, int wParam, int lParam);
[DllImportAttribute("user32.dll")]
public static extern bool ReleaseCapture();
and finally, we will write the code to send the message whenever the user presses the mouse button. The form will be repositioned as per the mouse movement if the user keeps the mouse button pressed.

C#
private void Form1_MouseDown(object sender, MouseEventArgs e)
{
    ReleaseCapture();
    SendMessage(this.Handle, WM_NCLBUTTONDOWN, HT_CAPTION, 0); 
}
Now, the form is borderless and movable too.

Points of Interest
There are many functions that can be used in a much more elegant manner by using the windows messages. Writing code ourselves could also be a good option but it is error prone and frankly "re-inventing the wheel". 

My article is in C# but the original article was meant for C++. but since it was basically for managed C++. The same code and philosophy will work without much/any changes.
