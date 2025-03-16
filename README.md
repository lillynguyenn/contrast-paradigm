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

% Initialize dependent variables - Lilly
rxnTime = zeros(NumberTrials,NumberPresentations);
detectionAccuracy = zeros(NumberTrials,NumberPresentations);
shapeAccuracy = zeros(NumberTrials,NumberPresentations);

% Define the on-screen text - Lilly
txtSize = 30;
screen('txtSize',window,txtSize);
txtColor = [0,0,0]

% Define response keys - Lilly
yess = 'y';
noo = 'n';
shapes = {'1','2','3','4'};

% Set up the experiment, number of presentations, and randomized shapes and colors - Lilly
for trial = 1:NumberTrials
for presentation = 1:NumberPresentations
if presentation <= NumberControl
isControl = true;
else
isControl = false;
end

if ~isControl
shapeldx = randi(4);
contrastldx = randi(length(ColorContrast));
shapeColor=shapeColors(shapeldx,:)*ColorContrast(contrastldx);
else
shapeColor=ControlColor;
end

% Establish the shapes - Lilly
squareSize = 200;
squareRect = CenterRectOnPointd([0,0,squareSize,squareSize]),screenXpixels/2,screenYpixels/2);
Screen('FillRect',window,backgroundGray,squareRect);
if ~isControl
switch shapes{shapeldx}
case 'Circle'
screen ('fillOval',window,shapeColor,squareRect);
case 'Square'
screen ('fillRect',window,shapeColor,squareRect);
case 'Cross'
screen ('drawLine',window,shapeColor,squareRect(1),squareRect(2),squareRect(3), squarerect(4),5);
screen ('drawLine',window,shapeColor,squareRect(1),squareRect(4),squareRect(3), squarerect(2),5);
case 'Triangle'
vertices = [squareRect(1),squareRect(4);squareRect(3),squareRect(4), (squareRect(1)+squareRect(3))/2,squareRect(2)];
screen ('fillPoly',window,shapeColor,vertices);
end
end

