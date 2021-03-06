# noxCTF 2018: WTF

__Tags:__ `crypto`, `crypto-rsa`, `crypto-substitution`  
__Total Solvers:__ 28  
__Total Points:__ 742

## Problem Statement

Um uhhhhhhhhh WTF IS THIS?! I give up. Now you try to solve this.

```python
N = lObAbAbSBlZOOEBllOEbblTlOAbOlTSBATZBbOSAEZTZEAlSOggTggbTlEgBOgSllEEOEZZOSSAOlBlAgBBBBbbOOSSTOTEOllbZgElgbZSZbbSTTOEBZZSBBEEBTgESEgAAAlAOAEbTZBZZlOZSOgBAOBgOAZEZbOBZbETEOSBZSSElSSZlbBSgbTBOTBSBBSOZOAEBEBZEZASbOgZBblbblTSbBTObAElTSTOlSTlATESEEbSTBOlBlZOlAOETAZAgTBTSAEbETZOlElBEESObbTOOlgAZbbOTBOBEgAOBAbZBObBTg

e = lBlbSbTASTTSZTEASTTEBOOAEbEbOOOSBAgABTbZgSBAZAbBlBBEAZlBlEbSSSETAlSOlAgAOTbETAOTSZAZBSbOlOOZlZTETAOSSSlTZOElOOABSZBbZTSAZSlASTZlBBEbEbOEbSTAZAZgAgTlOTSEBEAlObEbbgZBlgOEBTBbbSZAZBBSSZBOTlTEAgBBSZETAbBgEBTATgOZBTllOOSSTlSSTOSSZSZAgSZATgbSOEOTgTTOAABSZEZBEAZBOOTTBSgSZTZbOTgZTTElSOATOAlbBZTBlOTgOSlETgTBOglgETbT

c = SOSBOEbgOZTZBEgZAOSTTSObbbbTOObETTbBAlOSBbABggTOBSObZBbbggggZZlbBblgEABlATBESZgASBbOZbASbAAOZSSgbAOZlEgTAlgblBTbBSTAEBgEOEbgSZgSlgBlBSZOObSlgAOSbbOOgEbllAAZgBATgEAZbBEBOAAbZTggbOEZSSBOOBZZbAAlTBgBOglTSSESOTbbSlTAZATEOZbgbgOBZBBBBTBTOSBgEZlOBTBSbgbTlZBbbOBbTSbBASBTlglSEAEgTOSOblAbEgBAbOlbOETAEZblSlEllgTTbbgb
```

## Solution

Based on the variable names, we can infer that this is probably __RSA__. However, the numbers are either encoded or encrypted in some way.

A quick way to gain insight on what kind of encoding this might be is by looking at the unique characters that we are dealing with.

```python
alphabet = set(N + e + c)
print(len(alphabet))
>>> 10
print(''.join(alphabet))
>>> AblgBZSETO
```

Since the number of unique characters used is `10` then this probably means that there is just some mapping between the characters `0123456789` and `AblgBZSETO`.

One of the first mappings I tried just in sorted order
```python
print(''.join(sorted(alphabet)))
>>> ABEOSTZbgl # -> 0123456789
```

But this did not work. A key insight that you need to get is that some of the characters look similar actual numbers, like `B to 8` and `O to 0`. If we base the mapping based on how much they look alike we get the following mappings

```
0 -> O
1 -> l
2 -> Z
3 -> E
4 -> A
5 -> S
6 -> b
7 -> T
8 -> B
9 -> g
```

And based on that we can get the actual values for `N`, `e`, and `c`.

We start with a standard attack on `RSA` with [`RsaCtfTool.py`](https://github.com/Ganapati/RsaCtfTool) to get the flag.

`noxCTF{RSA_1337_10rd}`

## Implementation

 ```
 N = 'lObAbAbSBlZOOEBllOEbblTlOAbOlTSBATZBbOSAEZTZEAlSOggTggbTlEgBOgSllEEOEZZOSSAOlBlAgBBBBbbOOSSTOTEOllbZgElgbZSZbbSTTOEBZZSBBEEBTgESEgAAAlAOAEbTZBZZlOZSOgBAOBgOAZEZbOBZbETEOSBZSSElSSZlbBSgbTBOTBSBBSOZOAEBEBZEZASbOgZBblbblTSbBTObAElTSTOlSTlATESEEbSTBOlBlZOlAOETAZAgTBTSAEbETZOlElBEESObbTOOlgAZbbOTBOBEgAOBAbZBObBTg'
e = 'lBlbSbTASTTSZTEASTTEBOOAEbEbOOOSBAgABTbZgSBAZAbBlBBEAZlBlEbSSSETAlSOlAgAOTbETAOTSZAZBSbOlOOZlZTETAOSSSlTZOElOOABSZBbZTSAZSlASTZlBBEbEbOEbSTAZAZgAgTlOTSEBEAlObEbbgZBlgOEBTBbbSZAZBBSSZBOTlTEAgBBSZETAbBgEBTATgOZBTllOOSSTlSSTOSSZSZAgSZATgbSOEOTgTTOAABSZEZBEAZBOOTTBSgSZTZbOTgZTTElSOATOAlbBZTBlOTgOSlETgTBOglgETbT'
c = 'SOSBOEbgOZTZBEgZAOSTTSObbbbTOObETTbBAlOSBbABggTOBSObZBbbggggZZlbBblgEABlATBESZgASBbOZbASbAAOZSSgbAOZlEgTAlgblBTbBSTAEBgEOEbgSZgSlgBlBSZOObSlgAOSbbOOgEbllAAZgBATgEAZbBEBOAAbZTggbOEZSSBOOBZZbAAlTBgBOglTSSESOTbbSlTAZATEOZbgbgOBZBBBBTBTOSBgEZlOBTBSbgbTlZBbbOBbTSbBASBTlglSEAEgTOSOblAbEgBAbOlbOETAEZblSlEllgTTbbgb'

alphabet = 'OlZEASbTBg'
mapping = {v: str(i) for i, v in enumerate(alphabet)}

def decode(s):
	return int(''.join([mapping[e] for e in s]))

N = decode(N)
e = decode(e)
c = decode(c)

print('python RsaCtfTool.py -n {} -e {} --uncipher {}'.format(N, e, c))

# python RsaCtfTool.py -n 106464658120038110366171046017584728605432723415099799671398095113303220554018149888866005570730116293196252665770382258833879353944414043672822102509840890423260826373058255315521685967807858850204383823245609286166175687064317570157147353365780181201403742497875436372013183350667001942660780839408462806879 -e 18165674577527345773800436360005849487629584246818834218136555374150149407637407524285601002127374055517203100485286275425145721883636036574242949710753834106366928190387866524288552807173498852374689387479028711005571557055252495247965030797704485232834280077859527260792773150470416827810790513797809193767 --uncipher 50580369027283924057750666670063776841058648997085062866999922168619348147835294586026456440255964021397419618768574389303695295198185200651940566009361144298479342683804462799603255800822644178980917553507665174247302696908288887870589321087856967128660867568458719153439705061463984601603743261513119776696
# [+] Clear text : b'noxCTF{RSA_1337_10rd}'
```
