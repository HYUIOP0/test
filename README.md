local triggerText = "I must find a worthy keeper!" -- This is the trigger text that the player must say, you say it near the "Alix rig"
local rig = game.Workspace:FindFirstChild("Alix")
local prox = script.Parent
local part = game.Workspace:WaitForChild("sll")
local maxDistance = 60


-- I wanted to add sounds to this just for fun as the animation is playing to make it more cooler
local playerSound = Instance.new("Sound")
playerSound.SoundId = "rbxassetid://1839703786"
playerSound.Name = "acc"
playerSound.Looped = false
playerSound.Volume = 0.5

local rigSound = Instance.new("Sound")
rigSound.SoundId = "rbxassetid://1839703786"
rigSound.Name = "acc2"
rigSound.Looped = false
rigSound.Volume = 0.5

-- This just checks if the player is close to the rig so that the prompt won't be activated just from anywhere
function checkMaxDistance(player)
	if player.Character and player.Character:FindFirstChild("HumanoidRootPart") then
		local playerPosition = player.Character.HumanoidRootPart.Position
		local rigPosition = rig.HumanoidRootPart.Position
		local distance = (playerPosition - rigPosition).magnitude
		if distance > maxDistance then
			prox.Enabled = false
			print(player.Name .. " has not said it close to the rig")
		else
			prox.Enabled = true
		end
	end
end

if prox then
	prox.Enabled = false

	local animation1 = Instance.new("Animation")
	animation1.AnimationId = "rbxassetid://112052672914918" -- Animation for the player

	local animation2 = Instance.new("Animation")
	animation2.AnimationId = "rbxassetid://80256908262418" -- Animation for the rig

	game.Players.PlayerAdded:Connect(function(player)
		player.Chatted:Connect(function(message)
			if message == triggerText then
				prox.Enabled = true
			end
		end)
		end)
		
		-- Below just waits to see if the player has a character before running the function itself
		game.Players.PlayerAdded:Connect(function(player)
			
			player.CharacterAdded:Wait()

			
			player.Chatted:Connect(function(message)
				if message == triggerText then
					
					checkMaxDistance(player)
				end
			end)

		prox.Triggered:Connect(function(plr)
			if plr == player then 
				
				
				
				
				local partClone = part:Clone() -- This clones the sll part
				partClone.Size = Vector3.new(1, 1, 1)
				partClone.Anchored = false
				partClone.Shape = Enum.PartType.Ball  

				partClone.Parent = plr.Character

				local rightHand = plr.Character:FindFirstChild("RightHand") -- This finds the rightHand of the player
				if rightHand then
					local humanoid = plr.Character:FindFirstChild("Humanoid")
					if humanoid then
						local animTrack1 = humanoid:LoadAnimation(animation1)
						animTrack1.Looped = false -- I disabled looping as it generalyy looked bad.
						animTrack1:Play()
						
						playerSound.Parent = plr.Character
						playerSound:Play()
						print("Sound 1 has played")

						wait(5)
						playerSound:Stop()

						animTrack1.Ended:Connect(function()
							animTrack1:Stop()
							animation1:Destroy()
						end)

						wait(animTrack1.Length)

						local motor = Instance.new("Motor6D") -- This is so the part stays on the player's right hand.
						motor.Name = "PlayerRightHandMotor"
						motor.Part0 = rightHand
						motor.Part1 = partClone
						motor.C0 = CFrame.new(0, 0, 0)
						motor.C1 = CFrame.new(0, 0, 0)
						motor.Parent = rightHand

						print(rig.Name .. " has received a miraculous.") -- this prints the rig who has gotten a MC
					else
						warn("Humanoid not found in player's character!") -- I was having problems with the code earlier on, so I was debuggging them.
					end

					wait(1)
					
						game:GetService("Chat"):Chat(plr.Character, rig.Name .. ", This is the Miraculous of the rabbit which grants the power of evolution, Can I trust you?", Enum.ChatColor.White) -- this is what the player then says
					
				else
					warn("RightHand not found in player's character!")
				end

				if rig then
					local humanoid = rig:FindFirstChild("Humanoid")
					if humanoid then
						local animTrack2 = humanoid:LoadAnimation(animation2)
						animTrack2.Looped = false
						animTrack2:Play()

						-- This parents the part to the rig		
						rigSound.Parent = rig
						rigSound:Play()
						print("Sound 2 has played")
						
						wait(5) 
						rigSound:Stop() -- Stops the sound afer 5 seconds
						
						local part2 = Instance.new("Part")
						part2.Name = "MC"
						part2.Size = Vector3.new(1, 1, 1)
						part2.Shape = Enum.PartType.Ball
						part2.Parent = rig.RightHand

						local motor = Instance.new("Motor6D")
						motor.Name = "RigRightHandMotor"
						motor.Part0 = rig.RightHand
						motor.Part1 = part2
						motor.C0 = CFrame.new(0, 0, 0)
						motor.C1 = CFrame.new(0, 0, 0)
						motor.Parent = rig.RightHand
						
						-- So basically as the animation is playing for the rig they transform into their shirts and pants
						local shirt = Instance.new("Shirt")  
						shirt.ShirtTemplate = "rbxassetid://16211948180"  
						shirt.Parent = rig:FindFirstChild("UpperTorso")  

						local pants = Instance.new("Pants")  
						pants.PantsTemplate = "rbxassetid://8103261056033"  
						pants.Parent = rig:FindFirstChild("LowerTorso")
						
						shirt.Parent = rig:WaitForChild("UpperTorso")
						pants.Parent = rig:WaitForChild("LowerTorso")
						
						-- This is for what the rig is going to say after the animation plays
						
						if rig:FindFirstChild("Humanoid") then
							game:GetService("Chat"):Chat(rig.Head, "Thanks, Fluff Clockwise!", Enum.ChatColor.White)
						else
							warn("Rig has no humanoid!")
						end

						animTrack2.Ended:Connect(function()
							animTrack2:Stop()
							animation2:Destroy()
						end)
					else
						warn("No humanoid found in the rig!")
					end
				end
			end
		end)
	end)
else
	warn("ProximityPrompt not found!")
end


