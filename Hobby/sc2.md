# 1. An auto input program in C++.
- send or request resources from allies, in one-key.
- sparse troops, in one-key.
- use it in PUBG to make AK47 shoots like soilder-76.
 
# 2. An information share server/client with allies.
- share allies` resources information.
- share allies` screen position.
- I dont think I can debug sc2, maybe screen recogonization is ok.

# 3. Object Detection by OpenCV
 [tutorial](https://www.youtube.com/watch?v=vXqKniVe6P8)
 ```python
  import cv2 
  import numpy as np
  game_img = cv2.imread('farm.png', cv2.IMREAD_UNCHANGED)
  # needle_img is a doodad shown in the game image, we want find the information about it
  needle_img = cv2.imread('wheat.png', cv2.IMREAD_UNCHANGED)
  result = cv2.matchTemplate(game_img, needle_img, cv2.TM_CCOEFF_NORMED)
 ```
