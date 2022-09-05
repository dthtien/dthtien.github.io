In the two generals problem, we imagine two generals, each leading an army, who want to capture a city. The city's
defences are strong, and if only one of the two armies attacks, the army will be defeated. However, if both armies
attack at the same time, they will successfully caputure the city


|army 1 | army 2 | outcome |
| ----- | ------ | ------- |
| does not attach | does not attack | nothing happens |
| attacks | does not attack | army 1 defeated |
| does not attack | attacks | army 2 defeated |
| attacks | attacks| city captured |

Desired: army 1 attacks if and only if army 2 attacks

Two generals need to coordinate their attack plan. This is made difficult by the fact that the two armies are camped
some distance apart and they can only communicate by messenger. The messengers must pass through territory controlled by
the city, and so they are sometimes captured. Thus, a message sent by one general may or may not be received by the
other general, and the sender does not know whether their message got through, except by receiving an explicite reply
from the other party. if a genral does not receive any messages, it's impossible to tell whether this because the other
general didn't send any messages or because all messengers were captured.

## How should the generals decide?
  1. General 1 always attacks, even if no response received?
    -> send lots of messages to increase probability that one will get through
    -> if all are captured, general 2 does not know about the attack so general 1 lose
  2. General 1 only attacks if positive response from general 2 is recevied?
    -> General 1 is safe
    -> General 2 knows that general 1 will only attack if general 2's response get through
    -> now general 2 is the same situation as general 1 in the first option

  No common knowledge: the only way of knowing something is to communicate it.

## Distributed system reflection
  - There is no way for one node to have certainty about state of another node.
  - The only way how a node can know something is by having that knowledge communicated in a message.

### Example
- The shop and the credit card payment processing service communicate per RPC and some of these messages may be lost.
Nevertheless, the shop wants to ensure that it dispatches the goods only if they are paid for and it only charges the
customer card if the goods are dispatched.
- In practice, the only shopping example does not exactly match the two generals problem: in this scenario, it's safe
  for the payments service to always go ahead with a payment, because if the shop ends up not being able to dispatch the
    goods it can refund the payment. The fact that a payment is something that can be undone (unlike an army being
    defeated ) makes the problem solvable. If the communication between shop and payment service is interrupted, the
    shop can wait until the connection is restored, and then query the payment service to find out the status of any
    transactions whose outcome was unknown.
