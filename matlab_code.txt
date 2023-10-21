# Tumour-Visualization-Software
College Project : To accept an MRI image from the user and extract the part of the image with the tumour and highlighting it for better visualization.
function varargout = brain_tumor_final_(varargin)
% BRAIN_TUMOR_FINAL_ MATLAB code for brain_tumor_final_.fig
%      BRAIN_TUMOR_FINAL_, by itself, creates a new BRAIN_TUMOR_FINAL_ or raises the existing
%      singleton*.
%
%      H = BRAIN_TUMOR_FINAL_ returns the handle to a new BRAIN_TUMOR_FINAL_ or the handle to
%      the existing singleton*.
%
%      BRAIN_TUMOR_FINAL_('CALLBACK',hObject,eventData,handles,...) calls the local
%      function named CALLBACK in BRAIN_TUMOR_FINAL_.M with the given input arguments.
%
%      BRAIN_TUMOR_FINAL_('Property','Value',...) creates a new BRAIN_TUMOR_FINAL_ or raises the
%      existing singleton*.  Starting from the left, property value pairs are
%      applied to the GUI before brain_tumor_final__OpeningFcn gets called.  An
%      unrecognized property name or invalid value makes property application
%      stop.  All inputs are passed to brain_tumor_final__OpeningFcn via varargin.
%
%      *See GUI Options on GUIDE's Tools menu.  Choose "GUI allows only one
%      instance to run (singleton)".
%
% See also: GUIDE, GUIDATA, GUIHANDLES

% Edit the above text to modify the response to help brain_tumor_final_

% Last Modified by GUIDE v2.5 13-Jun-2018 14:05:12

% Begin initialization code - DO NOT EDIT
gui_Singleton = 1;
gui_State = struct('gui_Name',       mfilename, ...
                   'gui_Singleton',  gui_Singleton, ...
                   'gui_OpeningFcn', @brain_tumor_final__OpeningFcn, ...
                   'gui_OutputFcn',  @brain_tumor_final__OutputFcn, ...
                   'gui_LayoutFcn',  [] , ...
                   'gui_Callback',   []);
if nargin && ischar(varargin{1})
    gui_State.gui_Callback = str2func(varargin{1});
end

if nargout
    [varargout{1:nargout}] = gui_mainfcn(gui_State, varargin{:});
else
    gui_mainfcn(gui_State, varargin{:});
end
% End initialization code - DO NOT EDIT


% --- Executes just before brain_tumor_final_ is made visible.
function brain_tumor_final__OpeningFcn(hObject, eventdata, handles, varargin)
% This function has no output args, see OutputFcn.
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
% varargin   command line arguments to brain_tumor_final_ (see VARARGIN)

% Choose default command line output for brain_tumor_final_
handles.output = hObject;

% Update handles structure
guidata(hObject, handles);

% UIWAIT makes brain_tumor_final_ wait for user response (see UIRESUME)
% uiwait(handles.figure1);


% --- Outputs from this function are returned to the command line.
function varargout = brain_tumor_final__OutputFcn(hObject, eventdata, handles) 
% varargout  cell array for returning output args (see VARARGOUT);
% hObject    handle to figure
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Get default command line output from handles structure
varargout{1} = handles.output;



function edit1_Callback(hObject, eventdata, handles)
% hObject    handle to edit1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

% Hints: get(hObject,'String') returns contents of edit1 as text
%        str2double(get(hObject,'String')) returns contents of edit1 as a double


% --- Executes during object creation, after setting all properties.
function edit1_CreateFcn(hObject, eventdata, handles)
% hObject    handle to edit1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    empty - handles not created until after all CreateFcns called

% Hint: edit controls usually have a white background on Windows.
%       See ISPC and COMPUTER.
if ispc && isequal(get(hObject,'BackgroundColor'), get(0,'defaultUicontrolBackgroundColor'))
    set(hObject,'BackgroundColor','white');
end


% --- Executes on button press in pushbutton1.
function pushbutton1_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
[Filename,Pathname]=uigetfile('*.png','File selector');
name=strcat(Pathname,Filename);
a=imread(name);
imwrite(a,'output1.png');
set(handles.edit1,'string',name);
axes(handles.axes1);
imshow(a);


% --- Executes on button press in radiobutton1.
function radiobutton1_Callback(hObject, eventdata, handles)
% hObject    handle to radiobutton1 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
a=imread('output1.png');
hb=butterhp(a,15,1);
[r,w]=size(hb);
axes(handles.axes2);
% imshow(a);
a=a(1:r,1:w);
af = fftshift(fft2(a));
afhb= af.*hb;
afhbi = ifft2(afhb);
%ifftshow(afhbi);
imshow(afhbi);
% Hint: get(hObject,'Value') returns toggle state of radiobutton1


% --- Executes on button press in radiobutton2.
function radiobutton2_Callback(hObject, eventdata, handles)
% hObject    handle to radiobutton2 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)

a=imread('output1.png');
hb=butterhp(a,15,1);
[r,w]=size(hb);
a=a(1:r,1:w);
isp = imnoise(a,'salt & pepper',0.1);
axes(handles.axes2);
med = medfilt2(isp);
imshow(med);
% Hint: get(hObject,'Value') returns toggle state of radiobutton2


% --- Executes on button press in radiobutton3.
function radiobutton3_Callback(hObject, eventdata, handles)
% hObject    handle to radiobutton3 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
a=imread('output1.png');
hb=butterhp(a,15,1);
[r,w]=size(hb);
a=a(1:r,1:w);
isp = imnoise(a,'salt & pepper',0.1);
med = medfilt2(isp);
axes(handles.axes2);
T1 = med;
T2=118 ;
it = im2bw(T1,T2/255);
imshow(it);


% Hint: get(hObject,'Value') returns toggle state of radiobutton3


% --- Executes on button press in radiobutton4.
function radiobutton4_Callback(hObject, eventdata, handles)
% hObject    handle to radiobutton4 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
a=imread('output1.png');
hb=butterhp(a,15,1);
[r,w]=size(hb);
a=a(1:r,1:w);
isp = imnoise(a,'salt & pepper',0.1);
med = medfilt2(isp);
T1 = med;
T2=118 ;
it = im2bw(T1,T2/255);
BW = it;
C = ~BW;
D = -bwdist(C);
D(C)= - Inf;
L = watershed(D);
im = a;
im(L==0) = 0;
axes(handles.axes2);
imshow(im);

% Hint: get(hObject,'Value') returns toggle state of radiobutton4


% --- Executes on button press in radiobutton5.
function radiobutton5_Callback(hObject, eventdata, handles)
% hObject    handle to radiobutton5 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
a=imread('output1.png');
hb=butterhp(a,15,1);
[r,w]=size(hb);
a=a(1:r,1:w);
isp = imnoise(a,'salt & pepper',0.1);
med = medfilt2(isp);
T1 = med;
T2=118 ;
it = im2bw(T1,T2/255);
se = strel('disk',5);
k = imerode(it,se);
l = imresize(k,[500,500]);
z = a;
z(k)=255;
axes(handles.axes2);
imshow(l);

% Hint: get(hObject,'Value') returns toggle state of radiobutton5


% --- Executes on button press in pushbutton2.
function pushbutton2_Callback(hObject, eventdata, handles)
% hObject    handle to pushbutton2 (see GCBO)
% eventdata  reserved - to be defined in a future version of MATLAB
% handles    structure with handles and user data (see GUIDATA)
