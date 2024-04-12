---
sidebar_position: 8
---

# How to set up App Engine for Preventive Maintenance Schedule

## SLD Servers Configuration

1. Switch the 'Background Processing Date' on SLD Servers for your company to **ON**.

    ![Screenshot](./media/how-to-set-up-appengine-for-preventive-maintenance-schedule/sld-server.jpg)

2. When you do it for the first time, you can get an error saying that you must have the following statement in the Post-transaction procedure. After adding the code, you can turn the "Background Processing Date" to **ON**.

    ![Screenshot](./media/how-to-set-up-appengine-for-preventive-maintenance-schedule/image2021-4-27-14-37-52.png)

## Plugins configuration

1. Install **ProcessForce** and the [MI360 plugin](/docs/appengine/download/mi360).

    ![Screenshot](./media/how-to-set-up-appengine-for-preventive-maintenance-schedule/Plugins.jpg)

2. Activate **ProcessForce** and **MI360** for your company.

    ![Screenshot](./media/how-to-set-up-appengine-for-preventive-maintenance-schedule/image2021-4-27-14-45-43.png)

## Background Processing configuration

Preventive Maintenance Schedule based on **Time** set up:

    1. Go to the **PMScheduleTimeBasedJob** background processing and activate it for your company.

    ![Screenshot](./media/how-to-set-up-appengine-for-preventive-maintenance-schedule/image2021-4-28-9-36-11.png)

    ![Screenshot](./media/how-to-set-up-appengine-for-preventive-maintenance-schedule/image2021-4-28-9-38-42.png)

Preventive Maintenance Schedule based on **Meter** values set up.

    1. Go in to the **PMScheduleMeterBasedJob**, **PMScheduleTimeBasedJob**, and **SEventJob** background processing and activate it for your company.

        ![Screenshot](./media/how-to-set-up-appengine-for-preventive-maintenance-schedule/image2021-4-28-9-41-35.png)

Preventive Maintenance Schedule based on **Time** and **Meter** values set up:

    1. Go in to the **PMScheduleMeterBasedJob**, **PMScheduleTimeBasedJob**, and **SEventJob** background processing and activate it for your company.

    ![Screenshot](./media/how-to-set-up-appengine-for-preventive-maintenance-schedule/image2021-4-28-9-44-34.png)
