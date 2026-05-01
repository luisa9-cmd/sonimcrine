# - Assignment 5

# - Import Stuff
import random

# - Player's State
xpos = 0 # - Tracking horizantal movement
ypos = 0 # - Tracking vertical movement
health = 100 # - Tracking health. If health is <= 0, player dies.
gold = 0 # - Tracks currency.
gameon = True # - If game is still continued or not.

# - Player's Items
shovel = False # - Used to find key.
key = False # - Used to unlock any exit.

shovelevent = False # - When player finds shovel, it is set to true.
keyevent = False # - When player finds key, it is set to true.
rpsevent = False # - When player encounters RPS, it is set to true.
healevent = False # - When player buys items for health, it is set to true.
gambleevent = False # - When player buys items for health, it is set to true.

# - Introduction - Game Title Design
print("░▒▓███████▓▒░ ░▒▓██████▓▒░       ░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░░▒▓██████▓▒░░▒▓█▓▒░░▒▓█▓▒░       ░▒▓██████▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓████████▓▒░ ")
print("░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░")
print("░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░")
print("░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓████████▓▒░░▒▓██████▓▒░       ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░ ")
print("░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░          ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░     ")
print("░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░      ░▒▓█▓▒░░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░          ░▒▓█▓▒░░▒▓█▓▒░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░")
print("░▒▓█▓▒░░▒▓█▓▒░░▒▓██████▓▒░        ░▒▓█████████████▓▒░░▒▓█▓▒░░▒▓█▓▒░  ░▒▓█▓▒░           ░▒▓██████▓▒░ ░▒▓██████▓▒░   ░▒▓█▓▒░     ")

# - Game Loop
while gameon:
  # - Health
  if health <= 0:
    print("\nYou've been defeated, mission failed.")
    break

  # - Gold
  if gold <= -200:
    print("You've become broke, and reached the point of no return, mission failed.")
    break

  # - Player Stats Displayed
  print("\nYour current position is: " + "(" + str(xpos) + ", " + str(ypos) + ")")
  print("\nHP: " + str(health) + " | " + "Gold: " + str(gold))
  print("\nInventory: ", end = "")
  if shovel == True and key == False:
    print("Shovel")
  elif key == True and shovel == False:
    print("Key")
  elif shovel == True and key == True:
    print("Shovel, Key")
  if key == False and shovel == False:
    print("Nothing")
  else:
    print("")

  print("\n-------------------------------------------------------------------------")

  # - Player Movement and Boundaries
  move = input("\nNavigate using numbers on the numpad arrows (< or ∨ or > or ∧): ")
  if move == "4": # - Player moves left.
    if xpos > -3:
      xpos -= 1
    else:
      print("\nYou can't go that way.")
  elif move == "6": # - Player moves right.
    if xpos < 3:
      xpos += 1
    else:
      print("\nYou can't go that way.")
  elif move == "8": # - Player moves up.
    if ypos < 3:
      ypos += 1
    else:
      print("\nYou can't go that way.")
  elif move == "2": # - Player moves down.
    if ypos > 0:
      ypos -= 1
    else:
      print("\nYou can't go that way.")
  else:
    print("\nSorry, that is not a valid move. Please try again. ") # - Player isn't following an instruction, so it is inquired to retry.

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
    otherevent = random.choice(["wind", "gold", "trap", "nothing", "herbs"])
    if otherevent == "wind": # - First bad event
      health -= 5
      print("\nThe wind hit you.") 
    elif otherevent == "gold": # - First good event
      gold += 5
      print("\nYou found some gold!") 
    elif otherevent == "trap": # - Second bad event
      health -= 10
      print("\nYou fell for a trap...") 
    elif otherevent == "herbs": # - Second good event
      health += 5
      print("\nYou found some herbs, your wound slowly closes.")
    else: # - Neutral event
      print("\nThere's nothing here.") 
      


  
  
