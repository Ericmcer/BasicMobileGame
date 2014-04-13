local gameInfo = require("gameInfo")
local physics = require("physics")
physics.start()
physics.setGravity(0,0)
local storyboard = require("storyboard")
local scene = storyboard.newScene()

 @@@@@@@@@@@@@@@@@@@@@@@@@@@@@@

--MOVEMENT BUTTONS
local movingRight = false 
local movingLeft = false
local firstRun = true   --to make a part of collisionDetected run once
local removeBranch = true
local score = 0
local currentY = display.contentCenterY+150

 function moveLeft(event)
   if movingLeft == true then
   char.x = char.x - 4
     if char.x < -20 then
        char.x = -20
     end
  end
end

function moveRight(event)
   if movingRight == true then
    char.x = char.x + 4
    if char.x > 350 then
      char.x = 350
    end
  end
end

function setRight(event)
  if event.phase == "began" then
    movingRight = true
    char.xScale = -1
  elseif event.phase == "ended" then
    movingRight = false
   end
end

function setLeft(event)
  if event.phase == "began" then
    char.xScale = 1
    movingLeft = true
  elseif event.phase == "ended" then
    movingLeft = false
  end
end


--COLLISION DETECTION AND OBJECT CREATION
--#######################################################

function spawnNewObj(artFile,x,y,locationX,locationY)
  obj = display.newImageRect(artFile,x,y)
  obj.x = display.contentCenterX+locationX
  obj.y =  locationY
  obj.class="obj"
  physics.addBody(obj,"static", {isSensor=true})
  
  function obj:collision(e)
     
     timer.performWithDelay(1,function()
     objHit.x = obj.x
     objHit.y = obj.y
     objHit:play()
     display.remove(obj)
     
     end, 1)
     
    
    return true
  end
  obj:addEventListener("collision",obj)
  elements:insert(obj)
  return obj
end

--create cloud spawning algorithm 

function startObjectsSpawning()
    --clean up animation
    if firstRun then
      lastObj = spawnNewObj("hitObject.png",130,55,0,display.contentCenterY+150)
      firstRun = false
    else
    
    if char.y > 650 then
      gameInfo.setScore(score)
      if score > gameInfo.highscore then
        gameInfo.set(score)
      end
      storyboard.gotoScene("restart")
    end
    objHit.x = display.contentCenterX + 300
    objHit:pause()
    --find size of next object
    sizeX = 130 - score/5
    if sizeX < 50 then sizeX = 50 end
    sizeY = sizeX/130 * 55
    xLocation = math.random(-150,150)
    currentY = currentY - 50 - math.random(150)
    if currentY < -120 then currentY = -120 +math.random(30) end
    newObj = spawnNewObj("hitObject.png",sizeX,sizeY,xLocation,currentY) 
    end
   
    
    
    
    
end


function collisionDetected(event)
  --set up object spawn for first collision
 
  if event.phase == "began" then
    transition.to(char, {time = 1000, y=char.y-180})
    score = score + 1
    currentScore.text = "Score: " .. score
  end
end

function screenScroll()
  back1.y = back1.y+2
  back2.y = back2.y+2
  if(back1.y > 860.5)then
    back1.y = display.contentCenterY - 578
  end
  if back2.y > 860.5 then
  back2.y = display.contentCenterY - 578
  end
  currentY = currentY+5
  for a=elements.numChildren,1,-1 do
    elements[a].y = elements[a].y + 5
end
  
  
  if firstRun == false and removeBranch then
  branch.y = branch.y +4
    if branch.y > 600 then
      branch:removeSelf()
      removeBranch = false
    end
  end
  
end

function charFall()

char.y = char.y+6
end





--STORYBOARDS @@@@@@@@@@@@@@@@@@@@@@@@@@@@
function scene:createScene(event)

 local screen = self.view
  --CREATE BACKGROUNDS
 back1 = display.newImageRect("back1.png",320,580)
 back1.x = display.contentCenterX
 back1.y = display.contentCenterY
 screen:insert(back1)

  
 back2 = display.newImageRect("back2.png",320,580)
 back2.x = display.contentCenterX
 back2.y = display.contentCenterY-578
 screen:insert(back2)
 
 --CREATE GROUP TO HOLD ELEMENTS
  elements = display.newGroup()
  elements.anchorChildren = true
  elements.anchorX = 0
  elements.anchorY = 1
  elements.x = 0
  elements.y = 0
  screen:insert(elements)
 
 animationElements = display.newGroup()
 animationElements.anchorX = 0
 animationElements.anchorY = 1
 animationElements.x = 0
 animationElements.y = 0
 screen:insert(animationElements)
 
 --LEFT AND RIGHT BUTTONS AND START BUTTON
 startButton = display.newImageRect("gameRun.png",100,50)
 startButton.x = display.contentCenterX
 startButton.y = display.contentCenterY-100
 screen:insert(startButton)
 
 leftButton = display.newImageRect("leftButton.png",100,50)
 leftButton.x = display.contentCenterX-100
 leftButton.y = display.contentHeight -30
 leftButton.alpha = .6
 screen:insert(leftButton)
 
 rightButton = display.newImageRect("rightButton.png",100,50)
 rightButton.x =display.contentCenterX + 100
 rightButton.y = display.contentHeight-30
 rightButton.alpha = .6
 screen:insert(rightButton)
 
 
 --TREE BRANCH FOR STARTING ANIMATION
 
 branch = display.newImageRect("startPoint.png",150,40)
 branch.x = 260
 branch.y = display.contentCenterY + 122
 screen:insert(branch)
 
 
 --SHOW SCORE/HIGHSCORE
 currentScore = display.newText("Score: ".. tostring(score),display.contentCenterX+100,25,"Helvetica",15)
 currentScore:setTextColor(255,255,255)
 screen:insert(currentScore)
 
 highScore = display.newText("Highscore: ".. tostring(gameInfo.highscore),display.contentCenterX-100,25,"Helvetica",15)
 highScore:setTextColor(255,255,255)
 screen:insert(highScore)

 --Obj Hit animation, will be offscreen and move onscreen when needed
 
 r_options = 
 {
  width = 130,
  height = 55,
  numFrames = 2,
  sheetContentWidth = 260,
  sheetContentHeight = 55
 }
 
 objHitSheet = graphics.newImageSheet("objectFade.png",r_options)
 objHit = display.newSprite(objHitSheet,{name = "objAnimation",start = 1, count = 2,
 time = 300,loopCount = 1})
 objHit.x = display.contentCenterX + 400
 screen:insert(objHit)
 
 --CREATE CHARACTER
     p_options = 
  {
    -- Required params
    width = 80,
    height = 42,
    numFrames = 4,
    -- content scaling
    sheetContentWidth = 320,
    sheetContentHeight = 42,
  }
  
  charSheet = graphics.newImageSheet("charSprite.png",p_options)
  char = display.newSprite(charSheet,{name = "player",start=1,count = 4,time = 800})
  char.x = display.contentCenterX + 120
  char.y = display.contentCenterY + 85
  physics.addBody(char, "static", {density=.1, bounce=0.1, friction=1})
  
  char:play()
  screen:insert(char)
  
end


--@@@@@@@@@@@@@@@@LATER FUNCTIONS
--will run once when game is started:
--1.get rid of start button and its action listener*
--2. animate frog off platform
--3.activate buttons
--4. start screen scrolling, spawn first object right below frog, setfrog to dynamic  


function gameStarting()
startButton:removeEventListener("tap",gameStarting)
startButton:removeSelf()
startButton = nil
backScroll = timer.performWithDelay(50,screenScroll,-1)
transition.to(char,{time = 1500,x=char.x-150,y=char.y-100})
 
objTimer = timer.performWithDelay(1000,startObjectsSpawning,-1)
charFallTimer = timer.performWithDelay(10,charFall,-1)


leftButton:addEventListener("touch",setLeft)
rightButton:addEventListener("touch",setRight)
char.bodyType = "dynamic"


end





--@@@@@@@@@@@@@@@@
function scene:enterScene(event)
storyboard.removeScene("start")
startButton:addEventListener("tap",gameStarting)
char:addEventListener("collision", collisionDetected )
Runtime:addEventListener("enterFrame", moveLeft)
Runtime:addEventListener("enterFrame", moveRight)

end


--@@@@@@@@@@@@@@@@@
function scene:exitScene(event)
Runtime:removeEventListener("enterFrame",moveLeft)
Runtime:removeEventListener("enterFrame",moveRight)
leftButton:removeEventListener("touch", setLeft)
leftButton:removeEventListener("touch",setRight)
timer.cancel(backScroll)
timer.cancel(objTimer)
timer.cancel(charFallTimer)
end


--@@@@@@@@@@@@@@@@@
function scene:destroyScene(event)
end


scene:addEventListener("createScene", scene)
scene:addEventListener("enterScene", scene)
scene:addEventListener("exitScene", scene)
scene:addEventListener("destroyScene", scene)

