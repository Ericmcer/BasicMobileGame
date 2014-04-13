local gameInfo = require("gameInfo")

local storyboard = require("storyboard")
local scene = storyboard.newScene()

--------------------------------

function startGame(event)
  if event.phase == "ended" then
    storyboard.gotoScene("game")
  end
end

function upgradeScreen(event)
     if event.phase == "ended" then
    storyboard.gotoScene("upgrades")
     end
end


local moveRight = false
local function birdMove()
  
  if char.x > 50 and not moveRight then
    char.x = char.x - 2
  elseif char.x < 280 and moveRight then 
    char.x = char.x + 2
  else
  if moveRight then
  char.xScale = 1
  else 
  char.xScale = -1
  end
  moveRight = not moveRight
  
  end
end
function quit()
os.exit(0)
end


---------------- storyboard template functions
function scene:createScene(event)

  local screen = self.view
  
  background = display.newImageRect("startBackGround.png",320,580)
  background.x = display.contentCenterX
  background.y = display.contentCenterY
  screen:insert(background)
  
  title = display.newImageRect("gameTitle.png",160,50)
  title.x = display.contentCenterX
  title.y = display.contentCenterY - 170
  screen:insert(title)
  
  startButton = display.newImageRect("startButton.png",120,40)
  startButton.x = display.contentCenterX
  startButton.y = display.contentCenterY + 70
  screen:insert(startButton)
  
  upgradeButton = display.newImageRect("upgradeButton.png",120,40)
  upgradeButton.x = display.contentCenterX 
  upgradeButton.y = display.contentCenterY + 130
  screen:insert(upgradeButton)
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
  char.x = display.contentCenterX + 150
  char.y = display.contentCenterY
  char:play()
  screen:insert(char)
  moveTimer = timer.performWithDelay(20,birdMove,-1)
  
  --load highscore and points
  
  gameInfo.load()
  

  highScore = display.newText("Highscore: ".. tostring(gameInfo.highscore),display.contentCenterX,25,"Helvetica",20)
  highScore:setTextColor(255,255,255)
  screen:insert(highScore)
  
  quitButton = display.newImageRect("quitButton.png",120,40)
  quitButton.x = display.contentCenterX
  quitButton.y = display.contentCenterY + 180
  screen:insert(quitButton)
end

function scene:enterScene(event)
  storyboard.removeScene("restart")
  storyboard.removeScene("upgrades")
  startButton:addEventListener("touch",startGame)
  upgradeButton:addEventListener("touch",upgradeScreen)
  quitButton:addEventListener("tap",quit)
end

function scene:exitScene(event)
  startButton:removeEventListener("touch",startGame)
  upgradeButton:removeEventListener("touch",upgradeScreen)
  quitButton:removeEventListener("tap",quit)
  timer.cancel(moveTimer)
end

function scene:destroyScene(event)
end


scene:addEventListener("createScene", scene)
scene:addEventListener("enterScene", scene)
scene:addEventListener("exitScene", scene)
scene:addEventListener("destroyScene", scene)

return scene
