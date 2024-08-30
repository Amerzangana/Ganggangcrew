# Tester Hotel

### Run the project for testing
```
npm run install
npm run build
npm run start
```


### Alternatively, you can run the three commands as one
```
npm install && npm run build && npm run start
```
### Run the project for developement
```
npm run install
npm run serve
npm run dev
```

## How to use the API

If you want access the API directly, first run the project for testing or development

Next make a request to the `/api/login` with credentials that can be found in `/server/data.js`

If successful, you will recieve an auth token. This token must be used in all other API requests

To use the auth token, JSON encode the username and token and set them in the `X-User-Auth` header

Example:
```
X-User-Auth: {"username":"tester01","token":"4230d922ca04ca2c2e908821021ac105"}
```

To see the available API endpoints, look in `/server/api.js`
The paths found here must be prefixed with `http://localhost:3000/api`

## Run with docker
```
docker build -t my-vue-app .
docker run -d -p 3000:3000 my-vue-app

```


import { test, expect } from '@playwright/test';



test.beforeEach(async ({ page }) => {
  await page.goto('http://localhost:3000/login/');
  await expect(page.getByRole('heading', { name: 'Login' })).toBeVisible();

  await page.locator('input[type="text"]').fill("tester01");
  await page.locator('input[type="password"]').fill("GteteqbQQgSr88SwNExUQv2ydb7xuf8c");
  await page.getByRole('button', { name: 'Login' }).click();
  await page.waitForTimeout(10000);
  await expect(page.getByRole('heading', { name: 'Tester Hotel Overview'})).toBeVisible();
});

test('Log in', async ({ page }) => {
  await expect(page.getByRole('heading', { name: 'Tester Hotel Overview'})).toBeVisible();

});


test('Create Room', async ({ page }) => {

  await page.locator('#app > div > div > div:nth-child(1) > a').click();
  await page.waitForTimeout(5000);
  await expect(page.getByRole('heading', { name: 'Rooms'})).toBeVisible();
  await page.waitForTimeout(1000);
  await page.getByRole('link', { name: 'Create Room' }).click();
  await page.waitForTimeout(10000);
  await page.getByRole('combobox').click();
  await page.waitForTimeout(1000);
  await page.selectOption('#app > div > div:nth-child(2) > div:nth-child(1) > select', { label: 'Single' });
  await page.waitForTimeout(1000);
  await page.fill('#app > div > div:nth-child(2) > div:nth-child(2) > input[type=number]', '3');
  await page.fill('#app > div > div:nth-child(2) > div:nth-child(3) > input[type=number]', '3');
  await page.locator('.checkbox').click();
  await page.fill('#app > div > div:nth-child(2) > div:nth-child(5) > input[type=number]', '999');
  await page.locator('#app > div > div:nth-child(2) > div:nth-child(6) > select > option:nth-child(3)').click();
  await page.locator('#app > div > div.actions > a.btn.blue').click();
  await expect(page.getByRole('heading', { name: 'Floor 3, Room' })).toBeVisible();
  await expect(page.getByText('Category: single')).toBeVisible();
  await expect(page.getByText('Price: 999kr')).toBeVisible();
  await expect(page.getByText('Features: sea view')).toBeVisible();
  await page.getByRole('link', { name: 'Back' }).click();

});

test('Test Bills', async ({ page }) => {
 
  await page.locator('div').filter({ hasText: /^BillsTotal:/ }).getByRole('link').click();
  await expect(page.getByRole('heading', { name: 'Bills' })).toBeVisible();
  await page.getByRole('link', { name: 'Create Bill' }).click();
  await page.fill('#app > div > div:nth-child(2) > div:nth-child(1) > input[type=number]', " 1500 ");
  await page.locator('.checkbox').click();
  await page.getByText('Save').click();
  await expect(page.getByRole('heading', { name: 'ID: 2' })).toBeVisible();
  await expect(page.getByText('Value: 1500kr').first()).toBeVisible();
  await page.waitForTimeout(5000);
  
});

test('Create client', async ({ page }) => {

  await page.locator('div').filter({ hasText: /^ClientsNumber: 2View$/ }).getByRole('link').click();

  await page.getByRole('link', { name: 'Create Client' }).click();

  await expect(page.getByRole('heading', { name: 'New Client'})).toBeVisible();
  await page.fill('#app > div > div:nth-child(2) > div:nth-child(1) > input[type=text]', "Lena Handen");
  await page.fill('#app > div > div:nth-child(2) > div:nth-child(2) > input[type=email]', "Lena@Handen.se");
  await page.fill('#app > div > div:nth-child(2) > div:nth-child(3) > input[type=text]', "12345678");
  await page.getByText('Save').click()
  await expect(page.getByRole('heading', { name: 'Lena Handen'})).toBeVisible();
});
