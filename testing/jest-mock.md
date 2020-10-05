#### Javascript Mock

    // thumb-war.js
    import {getWinner} from './utils'
    function thumbWar(player1, player2) {
    const numberToWin = 2
    let player1Wins = 0
    let player2Wins = 0
    while (player1Wins < numberToWin && player2Wins < numberToWin) {
        const winner = getWinner(player1, player2)
        if (winner === player1) {
        player1Wins++
        } else if (winner === player2) {
        player2Wins++
        }
    }
    return player1Wins > player2Wins ? player1 : player2
    }
    export default thumbWar

* The simplest form of mocking is monkey-patching values. Here's an example of what our test looks like when we do that:

    import thumbWar from '../thumb-war'
    import * as utils from '../utils'
    test('returns winner', () => {
    const originalGetWinner = utils.getWinner
    // eslint-disable-next-line import/namespace
    utils.getWinner = (p1, p2) => p2
    const winner = thumbWar('Ken Wheeler', 'Kent C. Dodds')
    expect(winner).toBe('Kent C. Dodds')
    // eslint-disable-next-line import/namespace
    utils.getWinner = originalGetWinner
    })

First we have to import the utils module as a * import so we have an object that we can manipulate (NOTE: read that with a grain of salt! More on why this is bad later). Then we need to store the original function at the beginning of our test and restore it at the end so other tests aren't impacted by the changes we're making to the utils module.
Here is the mock
```utils.getWinner = (p1, p2) => p2```

It's effective (we're now able to ensure there's a specific winner of the thumbWar game), but there are some limitations to this. One thing that's annoying is the eslint warning, so we've disabled that (again, don't actually do this as it makes your code non-spec compliant! Again, more on this later). Also, we don't actually know for sure whether the utils.getWinner function was called as much as it should have been (twice, for a best 2 out of 3 game). This may or may not be important for the application, but it's important for what I'm trying to teach you so let's improve that!


#### Jest mock

    test('returns winner', () => {
    const originalGetWinner = utils.getWinner
    // eslint-disable-next-line import/namespace
    utils.getWinner = jest.fn((p1, p2) => p2)
    const winner = thumbWar('Ken Wheeler', 'Kent C. Dodds')
    expect(winner).toBe('Kent C. Dodds')
    expect(utils.getWinner).toHaveBeenCalledTimes(2)
    utils.getWinner.mock.calls.forEach(args => {
        expect(args).toEqual(['Ken Wheeler', 'Kent C. Dodds'])
    })
    // eslint-disable-next-line import/namespace
    utils.getWinner = originalGetWinner
    })

By default, Jest will just keep the original implementation of getWinner but still keep track of how it's called. For us though we don't want the original implementation to be called so we use mockImplementation to mock out what happens when it's called. Then at the end we use mockRestore to clean up after ourselves just as we were before.

When I import a module, I'm importing immutable bindings to the functions in that module,

to solve this, we could attempt to muck with the require.cacheto swap the actual implementation of the module for our mock version, but we'd find out that imports happen before our code runs and so we wouldn't be able to run it in time without pulling it into another file.

So now we come to the jest.mock API. Because Jest actually simulates the module system for us, it can very easily and seamlessly swap out a mock implementation of a module for the real one! Here's what our test looks like now:

    import thumbWar from '../thumb-war'
    import * as utilsMock from '../utils'
    jest.mock('../utils', () => {
    return {
        getWinner: jest.fn((p1, p2) => p2),
    }
    })
    test('returns winner', () => {
    const winner = thumbWar('Ken Wheeler', 'Kent C. Dodds')
    expect(winner).toBe('Kent C. Dodds')
    expect(utilsMock.getWinner).toHaveBeenCalledTimes(2)
    utilsMock.getWinner.mock.calls.forEach(args => {
        expect(args).toEqual(['Ken Wheeler', 'Kent C. Dodds'])
    })
    })

We just tell Jest we want all files to use our mock version instead. Notice also that I changed the name of the import from utils to utilsMock. That's not required, but I like doing that to communicate the intention that this should be importing a mocked version of the module, not the real thing.

What if we're using this getWinnerfunction in several of our tests and we don't want to copy/paste this mock everywhere? That's where the __mocks__ directory comes in handy! So we create a __mocks__ directory right next to the file that we want to mock, and then create a file with the same name: