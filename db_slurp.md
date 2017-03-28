# DB:Slurp

![123](http://media1.giphy.com/media/xPBIDawrdRUqc/giphy-downsized.gif)

The `rake db:slurp` command (available in MyRewards and GPS) imports a seed data file into your local development environment. 
It also updates all passwords to 'password'.

## Setup

To use Db:Slurp, you need to clone the DataSeed (`git@github.com:CorporateRewards/DataSeed.git`) repo into the same folder as MyRewards/GPS, e.g.
```
  projects/
  ├── myrewards/
  ├── gps/
  ├── DataSeed/
```

## Usage
Run the `rake db:slurp` command from the running app container, e.g.
```bash
  docker exec -it myrewards_myrewards.app_1 bash -c 'rake db:slurp'
```

DB:Slurp takes care of migrations, updates all user and admin passwords to 'password', and also performs a `rake db:test:prepare`.

## Updating
It's a good idea to update the slurp files from time-to-time. 

1. Use Sequel Pro to connect to the staging database.
2. File | Export
3. The `versions` table can be huge and we don't need the content, so scroll down to `versions` table and un-check the "C" column (C for Content).
4. Click 'Export'
5. Replace the `default.sql` file for the project (e.g. `cp ~/Desktop/myrewards_staging_2017-03-28.sql ~/code/DataSeed/myrewards/default.sql`)
6. Commit to master and push
