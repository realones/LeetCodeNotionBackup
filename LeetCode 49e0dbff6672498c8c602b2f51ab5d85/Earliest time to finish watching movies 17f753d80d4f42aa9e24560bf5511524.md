# Earliest time to finish watching movies

#: 9001
Difficult: Hard
Tags: Array, BS_lower_upper_bound, Sort
Priority: High
Status: No Idea At First, toRevisit
Created time: October 11, 2023 6:56 PM
Last edited time: November 11, 2023 5:27 PM
source: amazonOA
related: Find Indices With Index and Value Difference II (Find%20Indices%20With%20Index%20and%20Value%20Difference%20II%20a245ef0a128344398828dc78b3f502b8.md)

you can start watching a movie at the time it is release or later
if you begin a movie at time t, it ends at time t+duration
if the movie ends at time t+duration, the 2nd movie can start at that time, t+ duration, or later

there are two category of movies: `comedy` and `drama`
determine the earliest time you can finish at least one move from each category. the release schedule and durations of movies are provided.

example
comedyReleaseTime = [1,4]
comedyDuration = [3,2]
dramaRelaeseTime = [5,2]
dramaRelaeseTime = [2,2]

the result is 6.
e.g. start watching comedy movie1 at time t=1, and finish at t=4, then watch drama movie2 from t=4, finish at 6

each input array length: 1~1e5

Solution pseudo - not verified, but should be workable solution:

```cpp
Intui:

sort musics by {release, duration} for each category

two arrays, so: for each A find best B:
	for each cate1 movie
		we know the end time (t1) of it, 
		need to find the smallest end time (t2) for each available cate2 movies
	same handling for cate2 movies
================================
now we need to handle: given startT, find the smallest endT for all same cate movies.

candidate1: movies released after the startT
postfixEarliestFinishTime[curMovie.startTime] =
	min 
		curMovie.finishTime
		pxx[nextMoive.startTime]

candidate2: movies released before the startT
prefixSmallestDuration[curMovie.startTime] = 
	min
		curMovei.duration
		prefixSmallestDuration[preMovie.startTime]

then use BS to find last prefixSmallestDuration and first postfixEarliestFinishTime,
compare to get smallest endT for given startT

================================
Others
================================

	subquestion: watchingT == startT
	then solve
	watchingT >= startT
================================
subquestion: watchingT == startT

	step1: maintain [(startT, minDuration)]
	for each movie:
		startT=releaseT
		minDuration=min(minDuration, curDuration)

	step 2:transfer to endT
	maintain [(startT, endT)]
	for each movie:
		startT=startT
		endT=startT+minDuration
================================
now, handle watchingT >= startT case
	update [(startT, endT)]
	for each movie, from back to start:
		startT=startT
		endT=min(endT,nxtEndT)
================================
now, reprocess is complete, we can start processing:
for each cate1 movie
	use lower_bound to find the earliest starting time, then from the endT of that element
	res=min(res,endT)
```

Solution wrong during OA:

```cpp
using ii=pair<int,int>;

int minimumTimeSpent(vector<int> comedyReleaseTime, vector<int> comedyDuration, vector<int> dramaReleaseTime, vector<int> dramaDuration) {
    vector<ii> comedy, drama;
    for(int i=0;i<comedyReleaseTime.size();i++) comedy.emplace_back(comedyReleaseTime[i], comedyDuration[i]);
    for(int i=0;i<dramaReleaseTime.size();i++) drama.emplace_back(dramaReleaseTime[i], dramaDuration[i]);
    sort(comedy.begin(), comedy.end());
    sort(drama.begin(), drama.end());
    
    //comedy
    //back to front, get {(time, minLen)}
    vector<ii> comedyMinLen;
    int minLen=INT_MAX;
    for(auto [cr,cd]: comedy){//cr: comedy release, cd: comedy duration
        minLen= min(minLen, cd);
        comedyMinLen.emplace_back(cr,minLen);
    }
    
    //drama
    //back to front, get {(time, minLen)}
    vector<ii> dramaMinLen;
    minLen=INT_MAX;
    for(auto [dr,dd]: drama){
        minLen= min(minLen, dd);
        comedyMinLen.emplace_back(dr,minLen);
    }
    
    int res=INT_MAX;
    //comedy then drama
    for(auto [cr,cd]: comedy){
        int finishTime1 = cr+cd;
        auto itr = lower_bound(dramaMinLen.begin(), dramaMinLen.end(), ii{finishTime1,0});
        if(itr!=dramaMinLen.end())
            res=min(res, itr->first + itr->second);
    }
    //drama then comedy
    for(auto [dr,dd]: drama){
        int finishTime1 = dr+dd;
        auto itr = lower_bound(comedyMinLen.begin(), comedyMinLen.end(), ii{finishTime1,0});
        if(itr!=comedyMinLen.end())
            res=min(res, itr->first + itr->second);
    }
    
    return res;
}
```