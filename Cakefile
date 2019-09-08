{ readdirSync, unlink, watch, writeFile } = require 'fs'
{ exec } = require 'child_process'
CSON = require 'cson'

# Gets the names of all the `.cson` files in the `Sources/InputMethods` directory.
names = readdirSync "Sources/InputMethods", (err, files) => if err then throw err else files
	.filter (file) => /\.cson$/.test file
	.map (file) => (file.match /([^\/]*)\.cson$/)[1]

task name, "build #{name}", (->
	writeFile "./#{ name }.js",
		"export default #{ JSON.stringify CSON.parseCSONFile "Sources/InputMethods/#{ name }.cson" }\n"
		(err) -> throw err if err
) for name in names

task "build", "build all files", ->
	for name in names
		writeFile "./#{ name }.js",
			"export default #{ JSON.stringify CSON.parseCSONFile "Sources/InputMethods/#{ name }.cson" }\n"
			(err) -> throw err if err
	return

task "watch", "watch all files and rebuild when changed", ->
	for name in names
		do (name) ->
			watch "./Sources/InputMethods/#{ name }.cson", "utf8", (type) ->
				return unless type is "change"
				console.log "File `#{name}` changed, rebuilding..."
				writeFile "./#{ name }.js",
					"export default #{ JSON.stringify CSON.parseCSONFile "Sources/InputMethods/#{ name }.cson" }\n"
					(err) -> throw err if err
	return

task "clear", "remove built files", ->
	(unlink "./${ name }.js", ->) for name in names
