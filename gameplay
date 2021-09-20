# importing the random module so we can shuffle our deck of cards
    # at the beginning of every game
import random


# the Card class represents the playing cards
# Each card has a suit (hearts, diamonds, spades and clubs) and a value (ace to king)
class Card:
    def __init__(self, suit, value):
        self.suit = suit
        self.value = value

    # notes to self on __repr__():
    # both str() and repr() are used to get a string representation of an object
    # the __str__() function is supposed to return a human-readable string
    # (i.e. something that's primarily there to be output to the end user)
    # the __repr__() method is supposed to return an "official" string representation of the object,
        # which can be used to construct the object again
    # here, we're defining the __repr__ function in order to change how the card is displayed
        # when we call print on it. This function will return the value and the suit
    def __repr__(self):
        return " of ".join((self.value, self.suit))


class Deck:
    # when creating an instance of the Deck, we simply need to have a collection of every possible card
    # we do this using a list comprehension which contains lists of every suit and value.
    # We pass each combination over to initialization for our Card class to create 52 unique card instances
    def __init__(self):
        self.cards = [Card(s, v) for s in ["Spades", "Clubs", "Hearts", "Diamonds"]
                      for v in ["A", "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K"]]

    def shuffle(self):
        # we'll only  shuffle a deck which still has at least 2 cards in it (shuffling 1 or 0 cards is pointless)
        if len(self.cards) > 1:
            random.shuffle(self.cards)

    # deal the cards
    def deal(self):
        if len(self.cards) > 1:
            # we use the pop function on the array holding our cards, to return the top card to us
            # and remove it from the deck so it can't be dealt again
            return self.cards.pop(0)


# Hand class: all players have a  hand of cards,
    # and each hand is worth a numerical value based on the cards it contains
class Hand:
    def __init__(self, dealer=False):
        # initialising our object attributes (dealer, cards and value):
        self.dealer = dealer
        self.cards = []  # like the Deck, a Hand will hold its cards as a list of Card instances
        self.value = 0

    # add a card to the Hand
    def add_card(self, card):
        self.cards.append(card)

    # calculate the value of a Hand:
    def calculate_value(self):
        # we initialise the value of the hand to 0 and assume the player doesn't have an ace (bc these are a special
        # case)
        self.value = 0
        has_ace = False
        # then we loop through the instances and try to add their value as a number to the player's total
        for card in self.cards:
            if card.value.isnumeric():
                self.value += int(card.value)
            # if the card's value is not numerical, check if the card's an ace
            # if it is an ace, we begin by adding 11 to the hand's value and setting the has_ace flag to True
            else:
                if card.value == "A":
                    has_ace = True
                    self.value += 11
                else:  # i.e. if it's K, Q or J: add 10
                    self.value += 10
        # if this increase of 11 points for an ace brings the hand's value >21,
            # we make the ace worth 1 point instead (11-10)
        if has_ace and self.value > 21:
            self.value -= 10

    # call the calculate_value method to count the total points of the sum of the cards in the hand
    # then return that total value
    def get_value(self):
        self.calculate_value()
        return self.value

    # print each card in the hand, and also print the value of the player's hand
    # the dealer's first card is face down so we print 'hidden' instead
    def display(self):
        if self.dealer:
            print("hidden")
            print(self.cards[1])
        else:
            for card in self.cards:
                print(card)
            print(f"Value: {self.get_value()}")


# the bet class retrieves the money bet by the player & keeps a tally of the overall gain/loss
class Bet:
    def __init__(self):
        # start the bet tally off at 0, and initialise the attribute that will store this tally (ongoing_bet)
        self.ongoing_bet = 0

        # retrieve an initial bet for the first round from the user, and store in the bet attribute
        self.bet = int(input("Welcome to Blackjack. How much will you bet? "))
        while self.bet < 0:
            self.bet = int(input("Please input a non-negative integer. a bet of £0 is permitted: "))
        print(f"You have bet £{self.bet}. Let's play.")

    # if the player wins a round, add the bet for that round to the ongoing_bet tally
    def recalculate_ongoing_bet_loss(self):
        self.ongoing_bet -= self.bet

    # if the player loses a round, subtract the bet for that round from the ongoing_bet tally
    def recalculate_ongoing_bet_gain(self):
        self.ongoing_bet += self.bet

    # for all subsequent rounds after the first, remind player of bet tally
    # and ask how much they wan to bet for this upcoming round
    def bet_again(self):
        if self.ongoing_bet >= 0:
            self.bet = int(input(f"Up to this point, you have won £{self.ongoing_bet}. How much will you bet? "))
            while self.bet < 0:
                self.bet = int(input("Please input a non-negative integer. a bet of £0 is permitted: "))
            print(f"You have bet £{self.bet}")
        elif self.ongoing_bet < 0:
            self.bet = int(input(f"You are running a loss of £{-self.ongoing_bet}. How much will you bet? "))
            while self.bet < 0:
                self.bet = int(input("Please input a non-negative integer. a bet of £0 is permitted: "))
            print(f"You have bet £{self.bet}")


# the game class and main loop:
# to begin playing, we simply need to create an instance of this class
class Game:
    def __init__(self):
        # we start off our loop with a Boolean which will track whether we're still playing the game
        playing = True

        # place the first bet (which also sets up ongoing_bet counter)
        gambling = Bet()

        # as long as we're still playing, run this code:
        while playing:
            # we'll need a shuffled deck..
            self.deck = Deck()
            self.deck.shuffle()

            # ...and 2 hand instances (one for dealer and one for player)
            self.player_hand = Hand()
            self.dealer_hand = Hand(dealer=True)

            # use the range function to deal 2 cards each to the player and the dealer.
            # our deal() method will return a Card instance,
            # which is passed to the add_card method of our Hand instances
            for i in range(2):
                self.player_hand.add_card(self.deck.deal())
                self.dealer_hand.add_card(self.deck.deal())

            # now display the hands to the player
            print("\nYour hand is: ")
            self.player_hand.display()
            # add a small timer here? e.g of 1 second?
            print("\nDealer's hand is: ")
            self.dealer_hand.display()

            game_over = False
            while not game_over:
                player_has_blackjack, dealer_has_blackjack = self.check_for_blackjack()
                # if either of the Booleans are True, then we have a winner
                # we'll print the winner to the screen and continue (which will break us out of the game loop)
                if player_has_blackjack or dealer_has_blackjack:
                    game_over = True
                    self.show_blackjack_results(player_has_blackjack, dealer_has_blackjack, gambling)
                    # continue is a way to neatly exit a for or while loop
                    continue

                # if neither player has 21 points:
                choice = input("Please choose [Hit / Stick]: \n").lower()
                while choice not in ["h", "hit", "s", "stick"]:
                    choice = input("Please enter 'hit' or 'stick' (or H/S): ")
                if choice in ["hit", "h"]:
                    self.player_hand.add_card(self.deck.deal())
                    self.player_hand.display()
                    # dealer hits after player hits, if dealer has less than 17 points:
                    if self.dealer_hand.get_value() < 17:
                        self.dealer_hand.add_card(self.deck.deal())
                        print("Dealer has hit.\n")

                    if self.player_is_over():
                        print("You've gone bust. You lose!")
                        gambling.recalculate_ongoing_bet_loss()
                        game_over = True
                    elif self.dealer_is_over():
                        print(f"Dealer's hand is {self.dealer_hand.get_value()}")
                        print("Dealer has gone bust. You win!")
                        gambling.recalculate_ongoing_bet_gain()
                        game_over = True

                else:
                    player_hand_value = self.player_hand.get_value()
                    dealer_hand_value = self.dealer_hand.get_value()

                    print("\nFinal Results")
                    print(f"Your hand: {player_hand_value}")
                    print(f"Dealer's hand: {dealer_hand_value}")

                    if player_hand_value > dealer_hand_value:
                        print("You win!")
                        gambling.recalculate_ongoing_bet_gain()
                    elif player_hand_value < dealer_hand_value:
                        print("You lose!")
                        gambling.recalculate_ongoing_bet_loss()
                    else:
                        print("Tie!")

                    game_over = True

            again = input("Play again? [Y /N]: ").lower()
            while again not in ["y", "n"]:
                again = input("Please enter Y or N: ")
            if again == "n":
                print("Thanks for playing!")
                if gambling.ongoing_bet >= 0:
                    print(f"You reached close of play with a gain of £{gambling.ongoing_bet}.")
                elif gambling.ongoing_bet < 0:
                    print(f"You reached close of play with a loss of £{-gambling.ongoing_bet}.")
                playing = False
            else:
                gambling.bet_again()

    def player_is_over(self):
        return self.player_hand.get_value() > 21

    def dealer_is_over(self):
        return self.dealer_hand.get_value() > 21

    # 1st: check for blackjack: did either player get exactly 21 points?
    def check_for_blackjack(self):
        # we need to keep track of which player might have blackjack,
        # so we'll keep a Boolean for the player and the dealer
        player = False
        dealer = False
        # now check if either hand totals 21 (if so, their boolean is changed to True)
        if self.player_hand.get_value() == 21:
            player = True
        if self.dealer_hand.get_value() == 21:
            dealer = True

        return player, dealer

    def show_blackjack_results(self, player_has_blackjack, dealer_has_blackjack, gambling):
        if player_has_blackjack and dealer_has_blackjack:
            print("\nBoth players have blackjack! Draw!")
        elif player_has_blackjack:
            print("\nYou have blackjack! You win!")
            gambling.recalculate_ongoing_bet_gain()
        elif dealer_has_blackjack:
            print("\nDealer has blackjack! You lose.")
            gambling.recalculate_ongoing_bet_loss()


if __name__ == "__main__":
    game = Game()
