The code is optimized to run at 6.45fps

## Optimizations: 
* Use cv2 functions to find the keypoints, descriptors and finally match them
* After matching, use only 100 best matches for finding Homography matrix
* Use cv2 function (warpPerspective) for warping 
* Instead of storing all the frames in a video, read and process the frame and continue to next frame.

**_All these optimizations achieve the similar performance with the original algorithm_**

## Profiling
1. Pre-processing:
   * "book_frames": 0.30624961853027344
   * "movie_frames": 0.21116900444030762
2. findCorrespondences:
   * "grayscale_conversion": 0.08624720573425293,
   * "features": 5.0387938022613525,
   * "findMatches": 52.779277086257935,
3. findHomography:
   * "H_ransac": 9.506160974502563,
4. projection:
   * "frame_resize": 0.17992901802062988,
   * "warping": 8.966251611709595,
   * "uint8_output": 0.589153528213501,
5. Post-processing
* "write_frame": 0.6327536106109619

"total_time": 79.18380784988403 for 511 frames

Reasons findMatches takes 52s:
* FAST detector generates lots of keypoins (aprrox 4000)
* So, a lots of descriptors also need to be computed
* After finding matches for all these keypoints, they are sorted to get best 100 matches. This sorting is taking a lot of time in 52s.

In order to further reduce the latency, reduce the number of keypoints detected by FAST detector.

**Please copy these files to python folder and use them**
