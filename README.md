# o2switch-whitelisting
Add the runner IP in your o2switch whitelist for later remote access.

- Retrieve the runner IP (using [`haythem/public-ip@v1.3`](https://github.com/haythem/public-ip))
- URL encode your password
- Supports 2FA

## Input Options

| Key                 | Required | Default | Description            |
| ------------------- |--------- | ------- | ---------------------- |
| `o2switch_username` | `true`   |         | The o2switch username. |
| `o2switch_password` | `true`   |         | The o2switch password. |
| `o2switch_host`     | `true`   |         | The o2switch host.     |
| `otp_secret`        | `false`  | `null`  | The OTP secret if it is enabled the o2switch account. |
| `ip_to_keep`        | `false`  | `''`    | The IP to keep in the whitelist. |

### `o2switch_password`

⚠️ If you don't have 2FA enabled on your o2switch account, then you may know you must URL encode your password.
Hopefully, this action takes care of this for you, so you can leave your real password in the `o2switch_password` input.

## Example workflow

```yml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      # Your own steps here
      - name: Install dependencies or whatever you need to do

      - name: Whitelist IP address
        uses: d9beuD/o2switch-whitelisting@v1
        with:
          otp_secret: ${{ secrets.O2SWITCH_OTP_SECRET }}
          o2switch_username: ${{ secrets.O2SWITCH_USER }}
          o2switch_password: ${{ secrets.O2SWITCH_PASSWORD }}
          o2switch_host: ${{ secrets.O2SWITCH_HOST }}

      # Example deploy step
      - name: Deploy
        uses: easingthemes/ssh-deploy@v5.1.0
        with:
          SSH_PRIVATE_KEY: ${{ secrets.SSH_PRIVATE_KEY }}
          # Updates are based on file hash, not time + size
          ARGS: '-rlgoDzvc -i --delete'
          SOURCE: './dist/'
          REMOTE_HOST: ${{ secrets.O2SWITCH_HOST }}
          REMOTE_USER: ${{ secrets.O2SWITCH_USER }}
          TARGET: ${{ secrets.APP_PATH }}/
```

## Contributing

We would love for you to contribute to d9beuD/o2switch-whitelisting, pull requests are welcome !
