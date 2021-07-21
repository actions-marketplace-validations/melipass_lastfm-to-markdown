# last.fm to markdown

![banner](banner.png)

## 🤖 About this repo
This is a small project that I started because I wanted to have my last.fm weekly chart on my GitHub profile. I used GitHub Actions because they can be scheduled with cron jobs and you won't need to pass any sensitive information to modify the README.md file.

## 🎵 Example output, automatically updated every day
<!-- lastfm -->
<p align="center"><img src="https://lastfm.freetls.fastly.net/i/u/64s/9715f15b9c7646f7c6bd919cb029fa7c.png" title="Flying Lotus - You're Dead!"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/c173c6486e9841a8b148ae5030a1ebdf.png" title="Flying Lotus - Until the Quiet Comes"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/50227cde795b4702c7b4d7ddcf0b85ff.png" title="Flying Lotus - Cosmogramma"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/96f7f1c340b277238bc8ca3c8b7fc457.jpg" title="Florence + the Machine - How Big, How Blue, How Beautiful"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/5f4e88dde3d1480bc8bb4ce07568513e.jpg" title="Flica - Weekendary"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/8ba9c92901b94bd7ba5fceda9f595d5a.jpg" title="Dum Dum Girls - Only in Dreams"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/ae729c6e9f591d54807cd34a00f9755f.jpg" title="Flica - Sub:Side"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/ad20fc22f42ef2fdae2fc99273f9f01f.png" title="Florence + the Machine - High as Hope"> <img src="https://lastfm.freetls.fastly.net/i/u/64s/cdbdd328cdaa4fefcf0f37e490f29c26.png" title="Javiera Mena - Otra Era"> </p>

          
## 👩🏽‍💻 What you'll need
* A README.md file.
* Last.fm API key
  * Fill [this form](https://www.last.fm/api/account/create) to instantly get one. Requires a last.fm account.
* Set up a GitHub Secret called ```LASTFM_API_KEY``` with the value given by last.fm.
* Also set up a ```LASTFM_USER``` GitHub Secret with the user you'll get the weekly charts for.
* Add a ```<!-- lastfm -->``` tag in your README.md file, with two blank lines below it. The album covers will be placed here.

## Instructions
To use this release, add a ```lastfm.yml``` workflow file to the ```.github/workflows``` folder in your repository with the following code:
```diff
name: lastfm-to-markdown

on:
  schedule:
    - cron: '2 0 * * *'
  workflow_dispatch:

jobs:
  lastfm:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: lastfm to markdown
        uses: melipass/lastfm-to-markdown@v1.2
        with:
          LASTFM_API_KEY: ${{ secrets.LASTFM_API_KEY }}
          LASTFM_USER: ${{ secrets.LASTFM_USER }}
#         IMAGE_COUNT: 6 # Optional. Defaults to 10. Feel free to remove this line if you want.
      - name: commit changes
        continue-on-error: true
        run: |
          git config --local user.email "action@github.com"
          git config --local user.name "GitHub Action"
          git add -A
          git commit -m "Updated last.fm's weekly chart" -a

      - name: push changes
        continue-on-error: true
        uses: ad-m/github-push-action@v0.6.0
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}\
          branch: main
```
The cron job is scheduled to run once a day because Last.fm's API updates weekly chart data daily at 00:00, it's useless to make more than 1 request per day because you'll get the same information back every time. You can manually run the workflow in case Last.fm's API was down at the time, going to the Actions tab in your repository.

## 🚧 To do
* Allow users to choose the image size for the album covers.
* Feel free to open an issue or send a pull request for anything you believe would be useful.
