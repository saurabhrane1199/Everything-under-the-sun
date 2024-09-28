# JioCinema Handling of IPL Load

## MatchDay Routine
- Setup WarRooms
    - Members of JioCinema, Cloud Providers and Third Party
- Scaling Up of Infra (Compute & Database)
- On call teams go through their dashboards and metrics
- Debrief

## Planning

- Starts 3-4 Months before
- Audit review -> defining breaking point of the individual systems in terms of rps
- Frontend, CDN, Backend, Databases
- For 3rd party partners


## Graceful Degradation

- Dividing features in to P0,P1,P2
- Exponential Backoff for retries of API Failure -> something like CDMA
- Use feature flags to control them



- Autoscaling takes 30min-45mins to detect and scale



- Database is crucial

- No one formula fits all, every microservices is different

- Increasing TTL of caches of databases, stale response but less load on database

## Panic Modes

 Take a snapshot of what API response looks like and store in static storage for catalogue etc. Abstracts the problem from the USER

- touch me not policy for infra

- Scaling down after the match in a ladder pattern


## MultiCDN

- Reliability
- Thier limits on edge service
- MultiCDN Optimiser Service decides which CDN to go
- Cache Desgin and policy very important. Cache Offload




- Designs that dont need to be in realtime should not be in realtim

- Kafka prioritise P0