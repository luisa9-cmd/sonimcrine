# - Assignment 5

# - Editors Note
  # - I reccomend running the code on github, just download VS Studio. This is because Replit's launcher tends to make an error and can have very poor performance at specific times.

# - Import Stuff
import zipimport
import random
import time

# - Time
starttime = time.time()
totaltime = 600

# - Player's State
xpos = 0 # - Tracking horizantal movement
ypos = 0 # - Tracking vertical movement
health = 100 # - Tracking health. If health is <= 0, player dies.
gold = 0 # - Tracks currency.
gameon = True # - If game is still continued or not.
pressure = 0 # - Tracks the amount of pressure (having 100 pressure makes the player lose their sanity.)
corruption = 0 # - Player is crystallized and unable to move if corruption reaches 100.
food = 0 # - Tracks the user's hunger.
water = 0 # - Tracks the user's thirst.
stamina = 150 # - Tracks the user's stamina. Having no stamina means that you're unable to continue.

# - Player's Items
shovel = False # - Used to find key.
key = False # - Used to unlock any exit.

shovelevent = False # - When player finds shovel, it is set to true.
keyevent = False # - When player finds key, it is set to true.
rpsevent = False # - When player encounters RPS, it is set to true.
healevent = False # - When player buys items for health, it is set to true.
gambleevent = False # - When player buys items for health, it is set to true.
corruptflag = False
storyevent = 0 # - Will play an event after every few moves, to enhance gameplay.
visited = 0

# - Tracking Events
lastrest = -1
laststory = -1

# - Introduction - Game Title Design and Story
print("░▒▓███████▓▒░ ░▒▓██████▓▒░       ░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░░▒▓██████▓▒░░▒▓█▓▒░░▒▓█▓▒░       ░▒▓██████▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓████████▓▒░ ")
print("░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░")
print("░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░")
print("░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓████████▓▒░░▒▓██████▓▒░       ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░ ")
print("░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░          ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░     ")
print("░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░          ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░")
print("░▒▓█▓▒░░▒▓█▓▒░░▒▓██████▓▒░        ░▒▓█████████████▓▒░░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░           ░▒▓██████▓▒░ ░▒▓██████▓▒░   ░▒▓█▓▒░     ")
print("\nYou wake up alone underground with no memory of how you got there. The air is cold and still, and the only light comes from the feint dim coming from the rubble above you. There is no visible entrance nor exit, only a small space with none left to spare. It seems as if searching for a way out only leads you further and further into a spiral. The path seems to dim and change every now and then when you becoime unattentive to your surroundings. All you can rely on is yourself. Trust no one. Hear no evil, see no evil.")

# - Game Loop
while gameon:

  # - Timer Display
  elapsed = int(time.time() - starttime)
  remaining = totaltime - elapsed

  if remaining <= 0:
    print("\n[TIMER] 00:00")
    print("\nYou've been consumed by the corruption, you lost.")
    break

  minutes = remaining // 60
  secs = remaining % 60

  print(f"\r\nYou have {minutes:02}:{secs:02} before the [Corruption] is achieved.")

  # - Min-Max Dynamics
  pressure = max(0, pressure)
  corruption = max(0, corruption)
  health = max(0, health)

  # - Corruption
  if corruption >= 100:
    print("\nThe corruption overwhelms you completely..")
    print("\nYou've lost yourself into the abyss")
    break

  # - Pressure
  if pressure >= 100:
    print("\nYou can't take it anymore; the pressure has made you lose your sanity., mission failed.")
    break
    
  # - Health
  if health <= 0:
    print("\nYou've been defeated, mission failed.")
    break

  # - Gold
  if gold <= -200:
    print("\nYou've become broke, and reached the point of no return, mission failed.")
    break

  # - Stamina
  if stamina <= 0:
    print("\nYou've lost your grip, your physical body cannot keep up.")
    break

  # - Player Stats Displayed
  print("\nYour current position is: " + "(" + str(xpos) + ", " + str(ypos) + ")")
  print("\nHP: " + str(health) + " | " + "Gold: " + str(gold) + " | " + "Corruption: " + str(corruption) + " | " + "Pressure: " + str(pressure))
  print("\nFood: " + str(food) + " | " + "Water: " + str(water) + " | " + "Stamina: " + str(stamina))
  print("\nInventory: ", end = "")
  
  if shovel == True and key == False:
    print("Shovel")
  elif key == True and shovel == False:
    print("Key")
  elif shovel == True and key == True:
    print("Shovel, Key")
  else:
    print("Nothing")
    
  if pressure >= 80 or corruption >= 80:
    print("\nYou may be undergoing an immense problem.")

  print("\n-------------------------------------------------------------------------")

  # - Player Movement and Boundaries
  move = input("\nNavigate using numbers on the numpad arrows (< or ∨ or > or ∧): ")

  while move not in ["4", "6", "8", "2"]:
    print("\nThat is not a valid choice.")
    move = input("\nNavigate using numbers on the numpad arrows (< or ∨ or > or ∧): ")
    
  if move == "4": # - Player moves left.
    if xpos > -3:
      xpos -= 1
      pressure += 1
      visited += 1
      stamina -= 1
    else:
      print("\nYou can't go that way.")
  elif move == "6": # - Player moves right.
    if xpos < 3:
      xpos += 1
      pressure += 1
      visited += 1
      stamina -= 1
    else:
      print("\nYou can't go that way.")
  elif move == "8": # - Player moves up.
    if ypos < 3:
      ypos += 1
      pressure += 1
      visited += 1
      stamina -= 1
    else:
      print("\nYou can't go that way.")
  elif move == "2": # - Player moves down.
    if ypos > 0:
      ypos -= 1
      pressure += 1
      visited += 1
      stamina -= 1
    else:
      print("\nYou can't go that way.")
  else:
    print("\nSorry, that is not a valid move. Please try again. ") # - Player isn't following an instruction, so it is inquired to retry.

  # - Player Restoration Mechanics
  if (visited % 5) == 0 and visited != 0 and visited != lastrest:
    lastrest = visited
    
    print("\nYou've went through a bit, what would you like to do?")
    print("\n7 => Replenish your hunger (-2 Pressure")
    print("\n9 => Satiate your thirst (-2 Corruption)")
    print("\n1 => Make a wish to the gods (+10 Gold)")
    print("\n3 => Build endurance (+3 stamina)")

    decision = input("\nWhat will you choose?\n\n")

    while decision not in ["7", "9", "1", "3"]:
      print("\nThat is not a valid choice.")
      decision = input("\nWhat will you choose?\n\n")
    if decision == "7":
      if food > 0:
        pressure -= 2
        print("\nYou ate a good, hearty meal.")
      else:
        print("\nYou have no food.")
    elif decision == "9":
      if water > 0:
        corruption -= 2
        print("\nYou drank holy water. Your mind begins to ease.")
      else:
        print("\nYou have no water.")
    elif decision == "1":
      gold += 10
      print("\nCoins fall from the sky. Your prayers have been anwswered.")
    elif decision == "3":
      stamina += 3
      print("\nYou build endurance through exercise.")
    
    

  # - Story Event
  if (visited % 3) == 0 and visited != 0 and visited != laststory:
    laststory = visited
    story = random.randint(1, 15) # - Tracks how much user explored to tell story
    # - FYI, these texts are made to enhance story, however prompts were generated by ChatGPT.
    if story == 1:
      print("\nYou notice strange markings on the wall - symbols that almost seem familiar.")
    elif story == 2:
      pressure += 1
      print("\nYou hear screams, noises, and scratches from behind... It makes you feel eerie...")
    elif story == 3:
      health += 5
      corruption -= 5
      print("\nYou found an abandoned cabin... there seems to be some supplies inside!")
    elif story == 4:
      corruption += 5
      print("\nTurn back... this place is full of miasma...")
    elif story == 5:
      corruption -= 10
      print("\nYou arrive in a room that seems empty. You're thirsty, so you drink the water that was placed on the table. The corruption eases.")
    elif story == 6:
      print("\nSomething's wrong.")
    elif story == 7:
      print("\nYou see visions of your family, seemingly near yet so far...")
    elif story == 8:
      print("\nThere's no point. There is no escape. Just give up.")
    elif story == 9:
      gold -= 30
      health += 20
      print("\nYou meet an adventurer. Seeing your wound, he offers a package of bandages in exchange for 30 gold.")
    elif story == 10:
      corruption += 5
      print("\nYou accidentally inhaled miasma.")
    elif story == 11:
      corruption -= 20
      pressure -= 10
      print("\nYou find a cross. Grasping on to it, your mind seems to calm down as you pray for help.")
    elif story == 12:
      pressure += 5
      health += 10
      print("\nYou see someone die to a wolf. You run away successfully, but you're physically tired. The wolf died however, and drops its meat and fur you can use for protection.")
    elif story == 13:
      print("\nYou dream of an escape. A guiding light hints towards two specific coordinates.. (3, 2) and (-3, 2).")
    elif story == 14:
      pressure -= 5
      gold += 15
      print("\nYou pass by a wanderer. He gives you some gold in wishes for your success.")
    else:
      print("\nYou're bored. Might as well continue travelling.")
    
  # - Exit 1 (FAKE EXIT)
  if xpos == -3 and ypos == 2:
    print("\nLight shimmers from the door, as if you've found the exit.")
    decision = input("\nWill you cross this area? (yes/no): ")
    if decision == "yes":
      print("\nAs you approach the exit, the rubble from above collapses you in. The light is close, but death is even closer. You have lost the game.")
      print("\n-------------------------------------------------------------------------")
      break
    else:
      print("\nMaybe next time.")
      print("\n-------------------------------------------------------------------------")
      xpos += 1
      continue

  # - Exit 2 (REAL EXIT)
  if xpos == 3 and ypos == 2:
    if key == True and health >= 50 and gold >= 50:
      print("\nUpon unlocking the door, a glimer of light shifts. The clouds glimmer and mingle, however letting the sun in through the dark corridors. As you walk and walk, you see a small village. The home you once had, and the home that you once missed, all gone. You've won and been freed, but at what cost?")
      print("\n-------------------------------------------------------------------------")
      break
    else:
      print("\nThis area seems to be locked. It seems to need a key and require some... sacrifices...")

  # - Shovel Event
  if xpos == -2 and ypos == 1:
    if shovelevent == False:
      print("\nYou found a shovel.")
      shovel = True
      shovelevent = True
    else:
      print("\nYou've already explored this area.")

  # - Key Event
  if xpos == 3 and ypos == 0:
    if keyevent == False:
      # - If player has obtained the shovel, this will be displayed and the following will happen:
      if shovel == True:
        key = True
        keyevent = True
        print("\nYou found a key.")

      # - If player has not obtained the shovel, this text is displayed.
      else:
        print("\nSomething's buried under here... come back later?")

  # - RPS Event
  if xpos == 0 and ypos == 1:
    if rpsevent == False:
      verifyrps = input("\nDo you want to play a game? (yes/no): ")
      
      # - Player chooses to play the game
      if verifyrps == "yes":
        rps1 = random.choice(["rock", "paper", "scissors"])
        rps2 = random.choice(["rock", "paper", "scissors"])
        count = 0

        # - Round 1 of 2
        round1 = str(input("\nWill you choose rock, paper, or scissors?\n\n"))
        print("\nThe computer chose " + rps1 + ".")

        if rps1 == "scissors" and round1 == "rock":
          print("\nYou won this round!")
          count += 1
        elif rps1 == "paper" and round1 == "scissors":
          print("\nYou won this round!")
          count += 1
        elif rps1 == "rock" and round1 == "paper":
          print("\nYou won this round!")
          count += 1
        elif rps1 == "paper" and round1 == "rock":
          print("\nYou lost this round!")
        elif rps1 == "rock" and round1 == "scissors":
          print("\nYou lost this round!")
        elif rps1 == "scissors" and round1 == "paper":
          print("\nYou lost this round!")
        else:
          print("\nWe tied this round!")

        # - Round 2 of 2
        round2 = str(input("\nWill you choose rock, paper, or scissors?\n\n"))
        print("\nThe computer chose " + rps2 + ".")

        if rps2 == "scissors" and round2 == "rock":
          print("\nYou won this round!")
          count += 1
        elif rps2 == "paper" and round2 == "scissors":
          print("\nYou won this round!")
          count += 1
        elif rps2 == "rock" and round2 == "paper":
          print("\nYou won this round!")
          count += 1
        elif rps2 == "paper" and round2 == "rock":
          print("\nYou lost this round!")
        elif rps2 == "rock" and round2 == "scissors":
          print("\nYou lost this round!")
        elif rps2 == "scissors" and round2 == "paper":
          print("\nYou lost this round!")
        else:
          print("\nWe tied this round!")

        # - Determines the prize based on how much rounds have been won.
        if count == 2:
         gold += 30
        elif count == 1:
         gold += 0
        else:
          gold -= 30

        # - Displays number of rounds and prizes.
        print("\nYou won " + str(count) + " round(s).")
        if count == 2:
          print("\nYou earned 30 gold!")
        elif count == 1:
          print("\nYou earned nothing.")
        else:
          print("You lost 30 gold.")

        rpsevent = True

      # - Player chooses to not play the game.
      else:
        print("\nMaybe next time...")
        continue

    # - Disables player from reintiating the event.
    else:
      print("\nYou've already explored this area.")

  # - Heal Event
  if xpos == -3 and ypos == 3:
    if healevent == False:
      healpotion = input("\nDo you want to buy a healing potion for 20 gold? (yes/no): ")
      # - If player decides to pay for healing. 
      if healpotion == "yes":
        if gold >= 20:
          health += 20
          gold -= 20
          healevent = True
          print("\nYou restored 20 HP. Nice!")
        else:
          print("\nYou're broke. Go get a job or something...")
      else:
        print("\nMaybe next time..")
        xpos += 1
    else:
      print("\nYou've already bought something here.")

  # - Gamble Event
  if xpos == 1 and ypos == 2:
    if gambleevent == False:
      gmblchoice = input("\nA merchant wishes to gamble with you... (yes/no)")
      if gmblchoice == "yes":
        if gold >= 10:
          gold -= 10
          roll = random.choice(["win", "lose"])
          if roll == "win":
            gold += 30
            print("\nYou won 30 gold! The merchant looks quite annoyed...")
          else:
            gold -= 30
            print("\nYou lost 30 gold... The merchant laughs manaically...  ")
        else:
          print("\nYou're broke. Consider finding a job?")
      else:
        print("\nYou're smart. You might've gotten scammed.")
      gambleevent = True
    else:
      print("\nThe merchant dissapeared. It looked shady anyways...")
    
  # - Miscellenous Events
  if (xpos, ypos) in [(-2, 0), (-1, 0), (1, 0), (2, 0), (-3, 0), (-3, 1), (-1, 1), (1, 1), (2, 1), (3, 1), (-2, 2), (-1, 2), (0, 2), (2, 2), (-2,3), (-1, 3), (0, 3), (1, 3), (2, 3), (3, 3)]:
    otherevent = random.choice(["wind", "gold", "trap", "nothing", "herbs", "corrupt", "relief", "food", "water", "stamina"])
    
    if otherevent == "wind": # - First bad event
      health -= 5
      print("\nThe wind hit you.") 
      
    elif otherevent == "gold": # - First good event
      gold += 5
      print("\nYou found some gold!") 
      
    elif otherevent == "trap": # - Second bad event
      health -= 10
      print("\nYou fell for a trap...") 

    elif otherevent == "relief": # - Third good event
      pressure -= 5
      print("\nYou've calmed down a bit.")
      
    elif otherevent == "herbs": # - Second good event
      health += 5
      print("\nYou found some herbs, your wound slowly closes.")

    elif otherevent == "food": # - Replenish hunger
      food += 1
      print("\nYou found some food!")

    elif otherevent == "water": # - Satiate thirst
      water += 1
      print("\nYou found some water!")

    elif otherevent == "stamina": # - Build player endurance
      stamina += 5
      pressure -= 3
      print("\nYou're tired.. but at least you got something out of it I guess... you can't keep doing this though.")
      
    elif otherevent == "corrupt": # - Third bad event
      if corruptflag == False:
        timepenalty = random.randint(10, 30)
        starttime -= timepenalty
        corruptiongain = random.randint(5, 10)
        corruption += corruptiongain
        corruptflag = True
        print("\nThe corruption seeps into your mind.")
        print("\nTime dilates, and crystals form.")
      else:
        timepenalty = random.randint(1, 3)
        starttime -= timepenalty
        corruptiongain = random.randint(5, 10)
        corruption += corruptiongain
        print("\nThe corruption seeps into your mind.")
        print("\nTime dilates, and crystals form.")    
      
    else: # - Neutral event
      print("\nThere's nothing here.") 
