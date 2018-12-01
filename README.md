

https://stackoverflow.com/questions/6557467/can-git-ignore-a-specific-line

sed "s!<lastRun>.*</lastRun>!<lastRun>2018-12-01T00:00:00</lastRun>!g" < makeIndex.ts
