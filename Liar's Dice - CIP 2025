import random

NUM_DICE = 5 #at the start of each game, each player has 5 dice

def roll_dice():
    # Roll 5 six sided dice and return the result in list format.
    dice_rolls = [] # starts with an empty list
    for i in range(NUM_DICE):
        roll =  random.randint(1, 6)
        dice_rolls.append(roll)
    return dice_rolls

def show_dice(players, dice):
    # print the player's dice rolls
    print(players + "'s dice: " + str(dice))

def get_user_bet():
    while True:
        # Ask the user to enter a bet.
        print("Place your bet!")
        print("")

        # while True: #Loops the user's bet
        quantity_input = input("What's the quantity of dice with the same face value showing?: ")  
        face_value_input = input("What is the face value shown? (example: 1-6): ")

        if quantity_input.isdigit() and face_value_input.isdigit():
            # isdigit is used to make sure the input is a number
            quantity = int(quantity_input)
            # type casting from string to integer
            face_value = int(face_value_input)

            if quantity > 1 and quantity <= 15 and face_value >= 1 and face_value <= 6:
            # Because there are only 3 players, the max quantity of dice in play is 15. The minimum face value of a 6 sided die is 1, and the max is 6. 
            # Making sure all bets are valid. We don't want people guessing 100 dice showing one, that's ridiculous. =D
                return quantity, face_value
            else:
                print("Bet error. Please check your numbers. Quantity has to be between 1 and 15, and face value must be between 1 and 6.")
        else:
          print("Invalid input")
          print("")


def total_face_value(player_dice, face_value):
    # We want to know the total values of all dice cast in this match. There are 15 dice that need to be added.
    total = 0 
    for dice in player_dice.values():
        total += dice.count(face_value)
    return total

def computers_decision(current_quantity, current_face_value):
    # To make this simple, I'm having the computer randomly decide to raise the bet or challenge the previous bet. 
    choice = random.choice(["raise", "raise", "challenge"])

    if choice == "challenge":
        return "challenge", current_quantity, current_face_value
    
    else: # Raise
        new_quantity = current_quantity
        new_face_value = current_face_value

        # Computer to increase quantity or face value
        if current_face_value < 6:
            new_face_value += 1
        else:
            new_quantity += 1
        
        return "raise", new_quantity, new_face_value

def count_total_face_value(player_dice, face_value):
    # looks at all of the dice rolled to check how many match the face value
    count = 0 
    for dice in player_dice.values():
        for die in dice:
            if die == face_value:
                count += 1
    return count

def get_user_raise(current_quantity, current_face_value):
    #helper function for user raising the bet
    print("Current bet is " + str(current_quantity) + " dice showing " + str(current_face_value) + ".")
    print("You chose to raise the bet.")
    quantity = int(input("Enter the new quantity of dice: "))
    face_value = int(input("Enter the new face value (1 - 6): "))
    print("You raised the bet to " + str(quantity) + " dice showing " + str(face_value) + ".")
    return quantity, face_value

# reveal all dice and judge the challenged bet. If the challenge is valid, end the round.
def reveal_and_judge(player_dice, current_quantity, current_face_value, betting_player, challenger):
    print("")
    print("All dice are revealed!")
    print("")
    for player_name, dice in player_dice.items():
        print(player_name + "'s dice: " + str(dice))
    print("")

    total_face_value_count = count_total_face_value(player_dice, current_face_value)
    print("There are actually " + str(total_face_value_count) + " dice showing the value " + str(current_face_value) + ".")
    print("")

    if total_face_value_count >= current_quantity:
        print("💀" + challenger + " loses this round. The previous bet was valid.")
        return True
    else: 
        print("The challenger wins. " + betting_player + " loses this round.")
        return True
    
    return True #end the round (hopefully)

def setup_round(round_counter, user_player, computer1, computer2):
    print("")
    print("===== Round " + str(round_counter) + " =====")
    # Roll dice for all players
    # key = player, value = random dice roll
    player_dice = {
        user_player : roll_dice(),
        computer1 : roll_dice(),
        computer2 : roll_dice()
        }

    # Show the user's dice
    print("")
    print("Roll your dice!")
    show_dice(user_player, player_dice[user_player])


    print("")
    # User always starts the game
    # print("Hello, " + user_player + "!") Maybe will come back to this for first round only. 
    print("Please start the round with the first bet.")
    print("")
    return player_dice


def main():
    print("🎲🏴‍☠️ Let's play Liar's Dice! 🏴‍☠️🎲")
    print("")
    print("Rules: ")
    print("1. Roll your dice on the table, but keep them hidden under your cup!")
    print("2. Increase the bet's value or quantity each round.") 
    print("If you bet 3 fours, you're claiming that across all the dice rolled, at least 3 dice show the number 4.")

    user_player = "🐥 User"
    computer1 = "🤖 Karel"
    computer2 = "🐙 Davy Jones"
    
    players = [user_player, computer1, computer2]

    round_counter = 1
    max_rounds = 3

    while round_counter <= max_rounds:
        player_dice = setup_round(round_counter, user_player, computer1, computer2)

        # User places bet
        current_quantity, current_face_value = get_user_bet()
        print("")
        print("You bet there are at least " + str(current_quantity) + " dice showing the value of " + str(current_face_value) + ".")
        print("")
        last_bet_placed_by = user_player

        round_continues = True

        while round_continues:
            # computer 1 decision tree
            print("🤔🤖 Karel is thinking...")
            choice, new_quantity, new_face_value = computers_decision(current_quantity, current_face_value)

            if choice == "challenge":
                print("⚔️ Karel has challenged your bet!")
                print("")
                round_ended = reveal_and_judge(player_dice, current_quantity, current_face_value, last_bet_placed_by, computer1)
                if round_ended:
                    print("")
                    print("===== End of Round " + str(round_counter) + " =====") # acknowledge the end of the round before exiting loop
                    print("")
                    round_counter += 1 
                    round_continues = False
                    break

                ## Something to work on, if the user bets 15 quantity then Karel ups the quantity to 16. 
                
            else: # Raise
                print("💰 Karel raises the bet to " + str(new_quantity) + " dice showing " + str(new_face_value) + ".")
                current_quantity = new_quantity
                current_face_value = new_face_value
                # after a raise is chosen, the current quantity and value becomes the new ones
                last_bet_placed_by = computer1
            

            # Computer2 (Davy Jones) makes his decision
            print("")
            print("🤔🐙 Davy Jones is thinking...")

            choice, new_quantity, new_face_value = computers_decision(current_quantity, current_face_value)

            if choice == "challenge":
                print("⚔️ Davy Jones challenges the bet!")
                print("")
                round_ended = reveal_and_judge(player_dice, current_quantity, current_face_value, last_bet_placed_by, computer2)
                if round_ended:
                    print("")
                    print("===== End of Round " + str(round_counter) + " =====")
                    print("")
                    round_counter += 1
                    round_continues = False
                    break

            else: # Raise
                print("💰 Davy Jones raises the bet to " + str(new_quantity) + " dice showing " + str(new_face_value) + ".")
                current_quantity = new_quantity
                current_face_value = new_face_value
                last_bet_placed_by = computer2

            print("")

            # user turn again
            print(user_player + ", you're up!")
            print("The current bet is " + str(current_quantity) + " dice showing " + str(current_face_value) + ".")
            print("")
            print("Do you want to raise the bet or challenge it?")
            while True:
                player_choice = input("Type 'raise' or 'challenge': ").lower()
                if player_choice == "challenge":
                    print("")
                    print("⚔️ You challenged the bet!")
                    print("")
                    round_ended = reveal_and_judge(player_dice, current_quantity, current_face_value, last_bet_placed_by, user_player)
                    if round_ended:
                        print("")
                        print("===== End of Round " + str(round_counter) + " =====")
                        print("")
                        round_counter += 1
                        round_continues = False
                        break
                elif player_choice == "raise":
                    current_quantity, current_face_value = get_user_bet()
                    print("💰 You raise the bet to " + str(current_quantity) + " dice showing " + str(current_face_value) + ".")
                    last_bet_placed_by = user_player
                    print("")
                    break
                else:
                    print("Invalid choice. Please type 'challenge or 'raise'")

            print("")
    
    
    
    print("💥🎉💥 Game over! Thank you for playing. 🌞🌹🌹🌹")

# test commit to check VSCode editor

    '''

    Liar's Dice is a game from the movie, Pirates of the Caribbean: Dead Man's 
    Chest. 

    Each player has 5 dice. Each round the dice are rolled and the player 
    either makes a larger bet or challenges the last bet. 

    A larger bet can be either increasing the quantity or the value of the bet. 
    for example: a player can bet 3 threes (sum of 9), the next can bet 4 threes
    (sum of 12, upping the quantity) or 5 twos (sum of 10, upping the value of bet)
    
    Every dice roll is hidden under a cup. Each player can only see their own 
    dice. 

    If a player challenges the previous bet, all the dice are revealed. 

    If the previous bid is valid, the bidder wins. 

    If the previous bid is invalid, the challenger wins. 

    Dice rolling mechanism: use random number generator to simulate 
    dice rolls for each player. 

    Players will either need to challenge a bet or make a new bid. 
        while loop to cycle through all players
        if statement to determine the validity of a claim. 
        for computer player's randomized decision making, will make for faster
        rounds. 

    How to catch a lie?
        conditionals and loops to check player's inputs against actual sums
        if a guess exceeds a certain value or based on their dice's value ==
        challenge, OR make it random. Perhaps player 2 and 3 are random. Or
        a threshold perhaps. Both? 

    
    Lists to keep track of each player's dice. 
    
    The player who loses a round, will lose one of their dice. The last 
            player to retain a die is the winner. 

        
    Optional:

    Create visuals for the dice and cups. Draw simple shapes or use images to 
    represent them. Use static images that can be placed at a starting point on 
    the game board
        Add animations, like rolling dice or shifting dice (to view your dice)
        animation loop
        Mouse interaction to roll the dice?
    Scoring

    To Fix/ Improve:
    Karel can raise the quantity to 16, will need to fix this. Same with Davy Jones. 
    How fast the game progresses - slow it down

    '''
    
    


    


    

if __name__ == "__main__":
    main()