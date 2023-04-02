# black-jack-python-flask-webapp
# I need help with this code for a game of blackjack but I'm getting problems with the server and python, html, css files in vscode. I load server with an internal error

PYTHON:
import random
from flask import Flask, render_template
import os
app = Flask(__name__)
app.config['TEMPLATES_AUTO_RELOAD'] = True
# double up the backslashes in the file path
path = os.path.abspath('C:\\Users\\bccam\\Downloads\\bljack\\templates.html')
with open(path, 'r') as file:
    ...

# Define the deck of cards
suits = ['hearts', 'diamonds', 'clubs', 'spades']
ranks = ['2', '3', '4', '5', '6', '7', '8', '9', '10', 'J', 'Q', 'K', 'A']
deck = [(rank, suit) for suit in suits for rank in ranks]


# Define the value of each card
card_values = {'Ace': 11, '2': 2, '3': 3, '4': 4, '5': 5, '6': 6, '7': 7, '8': 8, '9': 9, '10': 10, 'Jack': 10, 'Queen': 10, 'King': 10}

# Define a function to calculate the value of a hand
def calculate_hand_value(hand):
    value = 0
    num_aces = 0
    for card in hand:
        rank = card[0]
        if rank == 'Ace':
            num_aces += 1
        value += card_values[rank]
    while num_aces > 0 and value > 21:
        value -= 10
        num_aces -= 1
    return value

# Define a function to deal cards
def deal_card(deck):
    return deck.pop(random.randint(0, len(deck)-1))

# Define the main game function
@app.route('/')
def index():
    # Initialize the deck and hands
    deck = [(rank, suit) for suit in suits for rank in ranks]
    random.shuffle(deck)
    player_hand = [deal_card(deck), deal_card(deck)]
    dealer_hand = [deal_card(deck), deal_card(deck)]
    return render_template('index.html', player_hand=player_hand, dealer_hand=dealer_hand)

@app.route('/hit', methods=['POST'])
def hit():
    player_hand = eval(request.form['player_hand'])
    deck = eval(request.form['deck'])
    player_hand.append(deal_card(deck))
    dealer_hand = eval(request.form['dealer_hand'])
    while calculate_hand_value(dealer_hand) < 17:
        dealer_hand.append(deal_card(deck))
    return render_template('index.html', player_hand=player_hand, dealer_hand=dealer_hand)

if __name__ == '__main__':
    app.run()
    
    HTML:
    
<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" type="text/css" href="css.css">
<script src="{{ url_for('static', filename='cardr.py') }}"></script>
    <title>Blackjack</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='style.css') }}">
  </head>
  <body>
    <div class="container">
      <h1>Blackjack</h1>
      <h2>Dealer:</h2>
      <div class="hand">
        <div class="card">{{ dealer_hand[0][0] }} of {{ dealer_hand[0][1] }}</div>
        <div class="card back"></div>
      </div>
      <h2>Player:</h2>
      <div class="hand">
        {% for card in player_hand %}
            <div class="card">{{ card[0] }} of {{ card[1] }}</div>
            {% endfor %}
            </div>
            <form method="POST" action="{{ url_for('hit') }}">
            <input type="hidden" name="deck" value="{{ deck }}">
            <input type="hidden" name="player_hand" value="{{ player_hand }}">
            <input type="hidden" name="dealer_hand" value="{{ dealer_hand }}">
            <input type="submit" value="Hit">
            </form>
            </div>
            
              </body>
            </html>
            ```
            CSS: 
            .container {
    margin: 0 auto;
    max-width: 600px;
  }
  
  h1 {
    text-align: center;
  }
  
  h2 {
    margin-top: 50px;
  }
  
  .hand {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
  }
  
  .card {
    width: 100px;
    height: 150px;
    margin: 10px;
    padding: 10px;
    border: 1px solid black;
    border-radius: 5px;
    text-align: center;
    font-size: 20px;
    font-weight: bold;
    background-color: white;
    box-shadow: 2px 2px 5px rgba(0, 0, 0, 0.25);
  }
  
  .card.back {
    background-image: url('https://i.imgur.com/nVUXhQs.png');
    background-size: cover;
  }
  
