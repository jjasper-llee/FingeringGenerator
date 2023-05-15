# FingeringGenerator

## Description:
Program that returns the best possible fingering for a given music.xml file. This will not only make it easier for beginners to play their instrument but will solve 2 important issues:

1. students don't have to wait for days until their music lesson to ask about fingerings 
2. alleviate the difficulty for underpriviledged students to play string instruments without a music teacher  

## How It Works:
The program reads a music.xml file (Figure 1.0) and scans for the "step" and "octave" that are the determinants of the note. In this example the note is "D2." Then, according to the table (Figure 1.1; work in progress) the note "D2" is converted into a number system (Figure 1.2). Immediately after, this note is added into a set of notes that represent a piece, which is then parsed into the optimization function (Figure 1.3) that returns our fingering! 

## How The Optimization Function Works:
This function takes in the set of notes (described above) and cycles through all the possible fingers that could play each note and calculates the "cost" or the difficulty to use each finger given the previous note played. 

### Figure 1.0:
`<note>
   <pitch>
      <step>D</step>
         <octave>2</octave>
   </pitch>
      <duration>1</duration>
      <type>quarter</type>
 </note>`
     
### Figure 1.1
notes_num_table = { 
 
    'C': {
      2: 0,
      3: 17
    },
    'D': {
      2: 2
    },
    'E': {
      2: 4
    },
    'F': {
      2: 5
    },
    'G': {
      2: 7
    },
    'A': {
      2: 9
    },
    'B': {
      2: 11
    }
}'

## Figure 1.2
The system is as follows for the cello:

'(Note #, Actual Note):
(0, C2)
(1, D2)
(2, E2)
(3, F2)
(4, F#2)
etc.'
      
## Figure 1.3
`def cello(notes):
    # initialize the dynamic programming table
    dp = [[None] * 4 for _ in range(len(notes))] # 4 is the number of fingers on a hand
    dp[0][0] = {'fingers': [1], 'cost': 0}
    dp[0][1] = {'fingers': [2], 'cost': 0}
    dp[0][2] = {'fingers': [3], 'cost': 0}
    dp[0][3] = {'fingers': [4], 'cost': 0}
    
    
    # fill in the dynamic programming table
    for i in range(1, len(notes)):
        for j in range(4):
            
            best_fingers = None
            best_cost = float('inf')
      
            for k in range(4):
              total_cost = 0
              print("count: " + str(k))
              cur_pos = find_pos(notes[i-1], k+1)
              cur_string = find_string(notes[i-1])
              num_half_steps = notes[i] - notes[i-1]

              try: #check if note belongs to cost_dict_stay; if so: do nothing; else: find cost
                  print("cur pos: " + str(cur_pos))
                  print("cur string: " + str(cur_string))
                  print("cur not: " + str(notes[i]))
                  print("new finger: " + str(j+1) + " old finger: " + str(k +1))
                  
                  
                  if(cur_pos != 0): 
                      f = cost_dict_stay[cur_pos][cur_string][notes[i]]
                      #total_cost = total_cost
                      print("cost = 0")
                      print("")
                      best_cost = total_cost
                      best_fingers = dp[i-1][k]['fingers'] + [f] # update the fingerings chosen to reach this note thus far
                      print("best fingers:" + str(best_fingers))
                  #else: break
                  # cost = 0 --> total cost remains

              except KeyError:
                  print("cur pos: " + str(cur_pos))
                  print("num half steps: " + str(num_half_steps))
                  print("")
                  c = cost_dict_shift[cur_pos][num_half_steps][(k+1, j+1)] # the cost of playing notes[i] with finger j having played notes[i-1] with finger k th
                  # I swapped b/c I didn't factor in shifting back from 1 to 2 for instance --> delete
                  total_cost = c + dp[i-1][k]['cost']
                  print("total cost: " + str(total_cost))
                  if total_cost < best_cost:
                    best_cost = total_cost # update the total cost of using the fingerings to reach this note thus far
                    print("best cost: " + str(best_cost))
                    print("-------------------------------")
                    best_fingers = dp[i-1][k]['fingers'] + [j+1] # update the fingerings chosen to reach this note thus far
            dp[i][j] = {'fingers': best_fingers, 'cost': best_cost}
    
    # find the optimal fingering
    best_fingers = dp[-1][0]['fingers']
    best_cost = dp[-1][0]['cost']
    for i in range(1, 4):
        if dp[-1][i]['cost'] < best_cost:
            best_fingers = dp[-1][i]['fingers']
            best_cost = dp[-1][i]['cost']
    
    return best_fingers`

_I'm a cellist myself, so I am starting with the instrument I'm most familiar with. I will, of course, add other instruments later_





