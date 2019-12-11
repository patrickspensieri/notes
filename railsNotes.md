# Ruby Notes

---

## Ruby version manager

### change ruby version
`rvm use 2.1.1`

### list installed versions (interpreters)
`rvm list`

### use sytem install ruby (as if no rvm installed)
`rvm system`

### uninstall version
`rvm uninstall 2.0.0`

---

## Rails

### start server
`rails server`

### start rails console
`rails console`

#### create new user
- `User.create!(email: 'test@gmail.com', password: '123', password_confirmation: '123')`
- `exit` to leave console

#### see table schema
- `User.column_names`

#### shell into database
`rails db`

### run migrations
`rake db:migration`
