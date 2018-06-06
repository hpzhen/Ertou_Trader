# Trading Project

This project uses reinforcement learning on stock market and agent tries to learn trading. The goal is to check if the agent can learn to read tape. 

## TODO list
- [ ] PyTorch version
- [ ] Code documents

## How to use
#### Steps to reproduce DQN
1. cd `tensor-reinforcement`;
2. Copy data from `https://drive.google.com/file/d/0B6ZrYxEMNGR-MEd5Ti0tTEJjMTQ/view and https://drive.google.com/file/d/0B6ZrYxEMNGR-Q0YwWWVpVnJ3YmM/view?usp=sharing` into `tensor-reinforcement` directory.
3. Create a directory `saved_networks` inside `tensor_reinforcement` for saving networks.
4. `python dqn_model.py`

#### Steps to reproduce PG
1. cd `tensor-reinforcement`;
2. Create a directory `saved_networks` inside `tensor_reinforcement` for saving networks.
3. `python pg_model.py`


## Introduction
### Policy network
I will be starting with simple feed-forward network. Though, I am also inclined to use convolutional network reason, they do very well when the minor change in input should not change ouput. For example: In image recognizition, a small pixel values change doesn't meam image is changed. Intutively stocks numbers look same to me, a small change should not trigger a trade but again the problem here comes with normalization. With normalization the big change in number will be reduced to a very small in inputs hence its good to start with feed-forward.

#### Feed-forward
I want to start with 2 layer first, yes that just vanilla but lets see how it works than will shift to more deeper network. On output side I will be using a sigmoid non-linear function to get value out of 0 and 1. In hidden layer all neurons will be RELU. With 2 layers, I am assuming that first layer w1 can decide whether market is bullish, bearish and stable. 2nd layer can then decide what action to take based on based layer.

### Training
I will run x episode of training and each will have y time interval on it. Policy network will have to make x*y times decision of whether to hold, buy or short. After this based on our reward I will label every decison whether it was good/bad and update network. I will again run x episode on the improved network and will keep doing it. Like MCTS where things average out to optimality our policy also will start making more positive decision and less negative decision even though in training we will see policy making some wrong choices but on average it will work out because we will do same thing million times.

### Episodic 
I plan to start with episodic training rather than continous training. The major reason for this is that I will not have to calculate reward after every action which agent will make which is complex to do in trading, I can just make terminal reward based on portfolio value after an entire episode (final value of portfolio - transaction cost occur inside the episode - initial value of portfolio). The other reason for doing it that I believe it will motivate agent to learn trading on episodes, which decreases risk of any outlier events or sentiment change in market.

This also means that I have to check the hypothesis on:
- Episodes of different length
- On different rewards terminal reward or rewards after each step inside an episode also.

More info here
https://docs.google.com/document/d/12TmodyT4vZBViEbWXkUIgRW_qmL1rTW00GxSMqYGNHU/edit

## Data sources
1. For directly running this repo, use this data source and you are all setup: `https://drive.google.com/open?id=0B6ZrYxEMNGR-MEd5Ti0tTEJjMTQ`
2. Nifty Data: `https://drive.google.com/folderview?id=0B8e3dtbFwQWUZ1I5dklCMmE5M2M&ddrp=1%20%E2%81%A0%E2%81%A0%E2%81%A0%E2%81%A09:05%20PM%E2%81%A0%E2%81%A0%E2%81%A0%E2%81%A0%E2%81%A0`
3. Nifty futures:`http://www.4shared.com/folder/Fv9Jm0bS/NSE_Futures`
4. Google finance 
5. Interative Brokers, I used IB because I have an account with them. 

For reading on getting data using IB
`https://www.interactivebrokers.com/en/software/api/apiguide/tables/historical_data_limitations.htm'`
`https://www.interactivebrokers.com/en/software/api/apiguide/java/historicaldata.html`
symbol: stock -> STK, Indices -> IND

## Reference
- https://github.com/lamuguo/deep_trader
- https://github.com/aikorea/awesome-rl