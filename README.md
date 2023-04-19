import random

options = ['rock', 'paper', 'scissors']

def print_player_decisions(player_decision_map):
  print("-------------------")
  for player, decision in player_decision_map.items():
    print("{}: {}".format(player, decision))
  print("-------------------")

def get_human_decision():
  decision = ""
  bad_input = True
  while bad_input:
    decision = str(input("Rock, Paper, or Scissors? ")).lower()
    bad_input = decision not in options
    if bad_input:
      print("bad input")
  return decision

def make_ai_decisions(player_decision_map):
  for player in player_decision_map.keys():
    if player != "Human":
      player_decision_map[player] = random.choice(options)

def get_who_made_decision(decision, player_decision_map):
  players = []
  for player, player_decision in player_decision_map.items():
    if player_decision == decision:
      players.append(player)
  return players

def calculate_losing_players(player_decision_map):
  rock_players = get_who_made_decision('rock', player_decision_map)
  paper_players = get_who_made_decision('paper', player_decision_map)
  scissors_players = get_who_made_decision('scissors', player_decision_map)

  # If there exists rock, paper, and scissors players, no one wins.
  if rock_players and paper_players and scissors_players:
    return []

  # If there are only rock and paper players, paper beats rock
  if rock_players and paper_players:
    return rock_players

  # If there are only paper and scissors players, scissors beats paper
  if paper_players and scissors_players:
    return paper_players

  # If there are only scissors and rock players, rock beats scissors
  if scissors_players and rock_players:
    return scissors_players
  
  # If there's just one group of players with a decision, or no decisions.
  return []

def eliminate_any_losing_players(player_decision_map):
  losing_players = calculate_losing_players(player_decision_map)
  if not losing_players:
    print("TIE, GO AGAIN\n")
    return
  
  linking_verb = "are"
  if len(losing_players) == 1:
    linking_verb = "is"
  print('{} {} eliminated.'.format(', '.join(losing_players), linking_verb))

  for losing_player in losing_players:
    del player_decision_map[losing_player]
  print('{} player(s) remain.\n'.format(len(player_decision_map)))

def rock_paper_scissors():
  number_of_ai = int(input("How many computer players? "))
  print("WELCOME TO ROCK, PAPER, SCISSORS. THERE ARE {} PARTICIPANTS PLAYING.".format(number_of_ai+1))
  
  player_decision_map = {"Human" : ""}
  for i in range(number_of_ai):
    player_decision_map["AI_" + str(i)] = ""

  game_is_over = False
  while not game_is_over:
    if "Human" in player_decision_map:
      player_decision_map["Human"] = get_human_decision()
    
    make_ai_decisions(player_decision_map)
    print_player_decisions(player_decision_map)
    eliminate_any_losing_players(player_decision_map)
    if len(player_decision_map) == 1:
      print("{} WINS!!!".format(list(player_decision_map.keys())[0]))
      game_is_over = True# rock_paper_scissors.py
