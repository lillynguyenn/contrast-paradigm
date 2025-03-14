# contrast-paradigm

% Clearing the workspace - Ava 
sca;
close all;

PsychDefaultSetup(2);

% Setting up the screen for the test - Ava
screens = Screen('Screens');
screenNumber = max(screens);

[window, windowRect] = PsychImaging('OpenWindow', screenNumber, white);
KbStrokeWait;
sca;
cmd-alt-esc;

[screenXpixels, screenYpixels] = Screen('WindowSize', window);
ifi = Screen('GetFlipInterval', window);
rr = FrameRate(window);

% Defining colors for the paradigms - Ava
white = WhiteIndex(screenNumber);
black = BlackIndex(screenNumber);
grey = white / 2;
backgroundGray = [128 128 128];
ShapeColors = [255 0 0; 0 255 0; 0 0 255; 255 255 0]; 
ControlColor = [0 0 0];

% Defining shapes for the paradigms - Ava 
shapes = {'Circle', 'Square', 'Triangle', 'Cross'};

% Defining the contrast levels btwn shapes from easy to hard - Ava
ColorContrast = linspace(0.1, 0.9, 10);

% Defining the parameters of our paradigms - Ava
NumberTrials = 4;
NumberPresentations = 15;
NumberControl = 5;
NumberExperimental = 10;


