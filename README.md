# o2switch-whitelisting
Add the runner IP in your o2switch whitelist for later remote access.

- Retrieve the runner IP (using [`haythem/public-ip@v1.3`](https://github.com/haythem/public-ip))
- Uses the o2switch **SshWhitelist API** (via cPanel API Token, no password needed)
- Supports 2FA

## Input Options

| Key                 | Required | Default | Description            |
| ------------------- |--------- | ------- | ---------------------- |
| `o2switch_username` | `true`   |         | The o2switch username. |
| `o2switch_api_token`| `true`   |         | The o2switch API token for authentication. |
| `o2switch_host`     | `true`   |         | The o2switch host.     |
| `otp_secret`        | `false`  | `null`  | The OTP secret if it is enabled the o2switch account. |
| `ip_to_keep`        | `false`  | `''`    | The IP to keep in the whitelist. |

### `o2switch_api_token`

Provide an API token generated from your o2switch cPanel for authentication.  
You can create and manage API tokens in your cPanel under "Manage API Tokens".  

For more details, see the [o2switch documentation](https://faq.o2switch.fr/cpanel/securite/token-api-cpanel/).

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
          o2switch_api_token: ${{ secrets.O2SWITCH_API_TOKEN }}
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
