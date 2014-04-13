local myData = require("mydata")
local gameInfo = require("gameInfo")
local storyboard = require("storyboard")
local scene = storyboard.newScene()

function scene:createScene(event)
 local screen = self.view
 
 currentScore = display.newText("Score: ".. tostring(gameInfo.getScore()),display.contentCenterX,100,"Helvetica",25)
 currentScore:setTextColor(255,255,255)
 screen:insert(currentScore)
 
 highScore = display.newText("Highscore: ".. tostring(gameInfo.highscore),display.contentCenterX,150,"Helvetica",25)
 highScore:setTextColor(255,255,255)
 screen:insert(highScore)
 
 exitScreen = display.newText("press to continue",display.contentCenterX,300,"Helvetica",30)
 exitScreen:setTextColor(255,255,255)
 screen:insert(exitScreen)
end
function leaveScene()
storyboard.gotoScene("start")
end

function scene:enterScene(event)
storyboard.removeScene("game")
gameInfo.save()
exitScreen:addEventListener("tap",leaveScene)
end

function scene:exitScene(event)
exitScreen:removeEventListener("tap",leaveScene)
end

function scene:destroyScene(event)
end




scene:addEventListener("createScene", scene)
scene:addEventListener("enterScene", scene)
scene:addEventListener("exitScene", scene)
scene:addEventListener("destroyScene", scene)

return scene
