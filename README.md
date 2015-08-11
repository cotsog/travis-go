# test project for go vendor builds

This Go package vendors a dependency (itself) which contains an unused package
("github.com/bpo/travis-go/unused"), which references an unavailable repository
("example.com/code/was/once/here"). When building Go code, we shouldn't care about this
dependency chain - because the unused package is unused.

Travis fetches dependencies with `go get -t -v ./...`, which fetches all
packages referenced by code in the current directory and its subdirectories. Go
1.5 vendor support has a special directory, `vendor`, which should not be
recursively pulled in this way. The workaround is to replace `go get -t -v
./...` with `go get -t -v $(go list ./... | grep -v /vendor/)`.

See [discussion on github](https://github.com/golang/go/issues/11659) for more details.
