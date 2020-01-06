### I should update this site more often :)

*This is mainly a note for myself...*

Usually it takes a long time between two of my posts,
so probably many Nikola releases appeared.

So before starting I think it is good idea to upgrade :p

What I do is something like this:

```bash
cd ale-rt.github.io.builder/
. bin/activate
pip install -U Nikola
pip freeze > requirements.txt
git diff
```

Yeah, I know, I should add this to a Makefile ;)

Then in one console I start nikola auto:

```bash
cd ale-rt
nikola auto
```

And on another one I add a new post:
```bash
nikola new_post -f markdown -2 -t "What to do when I want to write a new blog post"
```

After that, if everything goes right I push my commits.
