# contrast-paradigm

% Clearing the workspace - Ava 
sca;
close all;

PsychDefaultSetup(2);

% Setting up the screen for the test - Ava
screens = Screen('Screens');
screenNumber = max(screens);

[window, windowRect] = PsychImaging('OpenWindow', screenNumber, [128 128 128]);
KbStrokeWait;
sca;
cmdaltesc;

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

% Setup and debugging of code above - Ava (~5 hours setting up psych
% toolbox and learning functions/doing preliminary research, ~1-2 hours coding and debugging)
% For above setup, both partners did prelimiary research on visual accuity,
% contrast paradigms, and existent literature (~5 hours)

% Initialize dependent variables - Lilly
rxnTime = zeros(NumberTrials,NumberPresentations);
detectionAccuracy = zeros(NumberTrials,NumberPresentations);
shapeAccuracy = zeros(NumberTrials,NumberPresentations);

% Define the on-screen text - Lilly
txtSize = 30;
screen('txtSize',window,txtSize);
txtColor = [0,0,0];

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
end
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
squareRect = CenterRectOnPointd(([0,0,squareSize,squareSize]),screenXpixels/2,screenYpixels/2);
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

% Present the stimulus and begin recording RT - Ava
Screen('Flip', window);
T0 = ReactionTime;

% Ask if participants see a shape and wait until they respond - Ava
DrawFormattedText(window, 'Is there a shape within the square? (y/n)', 'center', screenYpixels/2 + squareSize/2 + 50, textColor);
Screen('Flip', window);
[~, keyCode] = KbWait;
RT = ReactionTime - T0;
key = KbName(find(keyCode));

% record RT in msec - Ava
RTfinal(trial, presentation) = ReactionTime * 1000; 

% Check accuracy of answer to y/n question - Ava
if (strcmpi(key, yesKey) && ~isControl) || (strcmpi(key, noKey) && isControl)
    detectionAccuracy(trial, presentation) = 1;
else
    detectionAccuracy(trial, presentation) = 0;
end

% Above section of code (from presenting stimulus to checking accuracy) -
% Ava (~2 hours)
% Run through and debugging/modification of all existent code (~2 hours)
