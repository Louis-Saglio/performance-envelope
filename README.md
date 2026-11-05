# performance-envelope

An agent skill that derives the theoretical speed of a program, then compares it with what the code actually does.

It applies Casey Muratori's back-of-the-envelope method: count the irreducible work, find the resource that bounds it, divide by that resource's throughput, and compare with the measurement. The result separates "the code is slow" from "the design asks for too much work".

## Who it is for

Anyone using a coding agent that reads `SKILL.md` files who wants a performance answer grounded in hardware limits instead of profiler folklore.

## Use

Ask for a "back-of-the-envelope calculation", "the theoretical speed", "how fast should this run", "its performance floor", or "is this fast or slow", and point the agent at the code or the measurement.

The agent returns a table with the work per unit, the binding resource, the floor, the observed value and the ratio, then a verdict and the single biggest lever.

## Documentation

- [Changelog](CHANGELOG.md)
- [User guide](docs/USER_GUIDE.md)
- [Developer guide](docs/DEVELOPER_GUIDE.md)

## License

[MIT](LICENSE)
