```
npm i node-schedule
npm i -D @types/node-schedule
```

```
* * * * * *
┬ ┬ ┬ ┬ ┬ ┬
│ │ │ │ │ │
│ │ │ │ │ └ day of week (0 - 7) (0 or 7 is Sun)
│ │ │ │ └───── month (1 - 12)
│ │ │ └────────── day of month (1 - 31)
│ │ └─────────────── hour (0 - 23)
│ └──────────────────── minute (0 - 59)
└───────────────────────── second (0 - 59, OPTIONAL)
```

```ts
import schedule from 'node-schedule';

// recurring time
// visit https://crontab.guru for help
schedule.scheduleJob('* * * * *', () => {
  console.log('this will run every 1 min');
});

// at particular time
const time = new Date(Date.now() + 5000);
schedule.scheduleJob(time, () => {
  console.log('This is run after 5 seconds!');
});

// start and end time
// eg, run after 5 seconds and stop after 10 seconds
const startTime = new Date(Date.now() + 5000);
const endTime = new Date(startTime.getTime() + 5000);
const job = schedule.scheduleJob({ start: startTime, end: endTime, rule: '*/1 * * * * *' }, () => {
  console.log('Time for tea!');
});

// shutdown jobs gracefully
schedule.gracefulShutdown();

// cancel job
jobName.cancel();
```
