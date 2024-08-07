# Installing the Prometheus plugin

#### 1. Copy the plugin folder

Copy the Prometheus plugin folder to `\plugins` folder inside the backstage project.

#### 2. Configure plugin

- Create a `.env` file in the root of the project and add the Prometheus Url and . . . credentials as shown bellow

  ```bash
  PROMETHEUS_URL="https://metrics-opsverse-demo-us.us-east4.gcp.opsverse.cloud"
  PROMETHEUS_CREDENTIAL="ZGV2b3Bzbm93OjlrYmdhNGJpaDg="
  ```

- Add proxy to the `app-config.yaml` by adding the following line in `proxy:`

  ```yaml
  proxy:
    endpoints:
      '/prometheus':
        target: ${PROMETHEUS_URL}
        changeOrigin: true
        headers:
          Authorization: 'Basic ${PROMETHEUS_CREDENTIAL}'
  ```

- Navigate to backend folder in package `package/backend` and run `yarn add dotenv`
- Modify the `index.ts` in the `backend/src` folder and add the following lines

  ```js
  import * as dotenv from 'dotenv';
  dotenv.config({ path: '../../.env' });
  ```

- To add the plugin route, navigate to `package/app/src` and modify the `App.tsx` and import the plugin page

  ```js
  import { PrometheusPage } from '@internal/backstage-plugin-prometheus';
  ```

  and add the route by adding

  ```js
  <Route path="/prometheus" element={<PrometheusPage />} />
  ```

  inside the `<FlatRoutes>..</FlatRoutes>`

- To add the plugin to the catalog component, navigate to `packages/app/src/components/catalog` and Modify the `EntityPage.tsx` to import the plugin

  ```js
  import { EntityPrometheusContent } from '@internal/backstage-plugin-prometheus/src/plugin';
  ```

  and add

  ```js
  <EntityLayout.Route path="/prometheus" title="Prometheus">
    <EntityPrometheusContent />
  </EntityLayout.Route>
  ```

  to the required `<EntityLayout>`
