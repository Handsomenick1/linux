# Assignment4

## result:
- [with ept](https://github.com/Handsomenick1/linux/blob/master/283assignment4/with_ept.png)
- [without ept](https://github.com/Handsomenick1/linux/blob/master/283assignment4/without_ept.png)

### What did you learn from the count of exits? Was the count what you expected? If not, why not?
  - With the output, we can see the Shadow paging includes more count of exits than Nested paging.
  - This is what I expected because the Shadow paging would exit when the VM tries to execute CR3, or occurs page faults and explicit TLB invalidations.
### What changed between the two runs (ept vs no-ept)?
  - After testing, I found that the speed of Nested paging is faster than Shadow paging, but the count of exits of Nested paging is less than Shadow paging.
