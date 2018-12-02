

https://stackoverflow.com/questions/6557467/can-git-ignore-a-specific-line


`sed "s!<lastRun>.*</lastRun>!<lastRun>2018-12-01T00:00:00</lastRun>!g" < makeIndex.ts`


in $HOME/.gitconfig file
```
[filter "lastRun-in-ts"]
  smudge = sed \"s!<lastRun>.*</lastRun>!<lastRun>2018-12-01T00:00:00</lastRun>!\"
  clean  = sed \"s!<lastRun>.*</lastRun>!<lastRun>2018-12-01T00:00:00</lastRun>!\"
```

In the command line, you want to type git config command as follows:
```
$ git config --global filter.lastRun-in-ts.smudge 'sed "s!<lastRun>.*</lastRun>!<lastRun>2018-12-01T00:00:00</lastRun>!"'
$ git config --global filter.lastRun-in-ts.clean  'sed "s!<lastRun>.*</lastRun>!<lastRun>2018-12-01T00:00:00</lastRun>!"'
```

Then you add `.gitattributes` file into the project directory:
```
*.ts filter=lastRun-in-ts
```
