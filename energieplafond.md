Energieplafond Dashboard
**Lovelace**

    views:
      - title: Home
        cards:
          - type: entities
            entities:
              - entity: sensor.electricity_meter_power_consumption
              - entity: sensor.verbruikvandaagtar1
              - entity: sensor.verbruikvandaagtar2
              - entity: sensor.verbruikvandaagtotaal
              - entity: input_number.verbruiklimietdag
              - entity: sensor.verbruik_dag_budget_op_basis_van_verbruik_rounded
              - entity: sensor.verbruik_prijs_indien_prijsplafond
            title: Vandaag
          - type: entities
            entities:
              - input_number.verbruikmelding1
              - input_number.verbruikmelding2
              - input_number.verbruikmelding3
              - input_number.verbruikmelding4
            title: Meldingen
          - type: gauge
            entity: sensor.verbruikvandaagtotaal
            needle: true
            name: Verbruik tot nu toe voor deze dag (7.3 kWh Max)
            severity:
              green: 0
              yellow: 6
              red: 7
            max: 10
          - type: entities
            entities:
              - entity: sensor.electricity_meter_energy_consumption_tarif_1
              - entity: sensor.electricity_meter_energy_consumption_tarif_2
              - entity: sensor.verbruik_meterstand_totaal
              - entity: sensor.verbruik_eind_stand_einde_contract_max
              - entity: input_number.verbruik_meterstand_begin_jaar
              - entity: input_number.verbruik_kwh_tot_eind_contract_budget
              - entity: sensor.verbruik_kwh_tot_contract_einde
              - entity: sensor.verbruik_dagen_tot_contract_einde
              - entity: sensor.verbruik_dag_budget_op_basis_van_verbruik
            title: Meterstanden
          - type: entities
            entities:
              - entity: sensor.verbruik_melding_1_kwh
              - entity: sensor.verbruik_melding_2_kwh
              - entity: sensor.verbruik_melding_3_kwh
              - entity: sensor.verbruik_melding_4_kwh
          - type: entities
            entities:
              - counter.verbruikoververbruikaantal
            title: Aantal dagen teveel verbruikt
          - type: iframe
            url: >-
              https://public.flourish.studio/visualisation/12168387/?utm_source=showcase&utm_campaign=visualisation/12168387
            aspect_ratio: 50%
          - type: entities
            entities:
              - entity: sensor.verbruik_sluimer_24u
              - entity: sensor.verbruik_sluimer_inschatting_24u
            title: Sluimerverbruik (laagste verbruik in 24u)

**Automations**

    - id: '1672658578154'
    
    alias: Verbruik Telegram Meldingen
    
    description: ''
    
    trigger:
    
    - platform: template
    
    value_template: '{{ ((states(''sensor.verbruikvandaagtotaal'')|float) > (states(''sensor.verbruik_melding_1_kwh'')|float))
    
    }}'
    
    id: melding1
    
    - platform: template
    
    value_template: '{{ ((states(''sensor.verbruikvandaagtotaal'')|float) > (states(''sensor.verbruik_melding_2_kwh'')|float))
    
    }}'
    
    id: melding2
    
    - platform: template
    
    value_template: '{{ ((states(''sensor.verbruikvandaagtotaal'')|float) > (states(''sensor.verbruik_melding_3_kwh'')|float))
    
    }}'
    
    id: melding3
    
    - platform: template
    
    value_template: '{{ ((states(''sensor.verbruikvandaagtotaal'')|float) > (states(''sensor.verbruik_melding_4_kwh'')|float))
    
    }}'
    
    id: melding4
    
    condition: []
    
    action:
    
    - choose:
    
    - conditions:
    
    - condition: trigger
    
    id: melding1
    
    sequence:
    
    - service: notify.helentychotelegram
    
    data:
    
    message: '{{ states(''sensor.verbruikvandaagtotaal'') }}kWh/{{ states(''input_number.verbruiklimietdag'')
    
    }}kWh limiet Dit is {{ states(''input_number.verbruikmelding1'') | float
    
    * 100  }}%'
    
    title: "Verbruik alert \U0001F50C"
    
    - conditions:
    
    - condition: trigger
    
    id: melding2
    
    sequence:
    
    - service: notify.helentychotelegram
    
    data:
    
    message: '{{ states(''sensor.verbruikvandaagtotaal'') }}kWh/{{ states(''input_number.verbruiklimietdag'')
    
    }}kWh limiet Dit is {{ states(''input_number.verbruikmelding2'') | float
    
    * 100  }}%'
    
    title: "Verbruik alert \U0001F50C"
    
    - conditions:
    
    - condition: trigger
    
    id: melding3
    
    sequence:
    
    - service: notify.helentychotelegram
    
    data:
    
    message: '{{ states(''sensor.verbruikvandaagtotaal'') }}kWh/{{ states(''input_number.verbruiklimietdag'')
    
    }}kWh limiet Dit is {{ states(''input_number.verbruikmelding3'') | float
    
    * 100  }}%'
    
    title: "Verbruik alert \U0001F50C"
    
    - service: counter.increment
    
    data: {}
    
    target:
    
    entity_id: counter.verbruikoververbruikaantal
    
    - conditions:
    
    - condition: trigger
    
    id: melding4
    
    sequence:
    
    - service: notify.helentychotelegram
    
    data:
    
    message: '{{ states(''sensor.verbruikvandaagtotaal'') }}kWh/{{ states(''input_number.verbruiklimietdag'')
    
    }}kWh limiet Dit is {{ states(''input_number.verbruikmelding4'') | float
    
    * 100  }}%'
    
    title: "Verbruik alert \U0001F50C"
    
    mode: single

**Helpers**
Made thru UI so not really importable:

    {
    
    "version": 1,
    
    "minor_version": 1,
    
    "key": "input_number",
    
    "data": {
    
    "items": [
    
    {
    
    "min": 0.0,
    
    "max": 15.0,
    
    "name": "VerbruikLimietDag",
    
    "step": 0.1,
    
    "icon": "mdi:chart-donut",
    
    "unit_of_measurement": "kWh",
    
    "mode": "slider",
    
    "id": "verbruiklimietdag"
    
    },
    
    {
    
    "id": "verbruikmelding1",
    
    "min": 0.0,
    
    "max": 1.0,
    
    "name": "VerbruikMelding1",
    
    "step": 0.01,
    
    "mode": "slider",
    
    "unit_of_measurement": "%"
    
    },
    
    {
    
    "id": "verbruikmelding2",
    
    "min": 0.0,
    
    "max": 1.0,
    
    "name": "VerbruikMelding2",
    
    "unit_of_measurement": "%",
    
    "step": 0.01,
    
    "mode": "slider"
    
    },
    
    {
    
    "id": "verbruikmelding3",
    
    "min": 0.0,
    
    "max": 1.0,
    
    "name": "VerbruikMelding3",
    
    "unit_of_measurement": "%",
    
    "step": 0.01,
    
    "mode": "slider"
    
    },
    
    {
    
    "id": "verbruikmelding4",
    
    "min": 0.0,
    
    "max": 1.5,
    
    "name": "VerbruikMelding4",
    
    "unit_of_measurement": "%",
    
    "step": 0.01,
    
    "mode": "slider"
    
    },
    
    {
    
    "min": 0.0,
    
    "max": 2900.0,
    
    "name": "Verbruik kWh tot eind contract budget",
    
    "unit_of_measurement": "kWh",
    
    "mode": "box",
    
    "step": 1.0,
    
    "id": "verbruik_kwh_tot_eind_contract_budget"
    
    },
    
    {
    
    "id": "verbruik_meterstand_begin_jaar",
    
    "min": 0.0,
    
    "max": 30000.0,
    
    "name": "Verbruik Meterstand Begin Jaar",
    
    "unit_of_measurement": "kWh",
    
    "step": 1.0,
    
    "mode": "box"
    
    }
    
    ]
    
    }
    
    }

**Template Sensors**

    template:
    
    - sensor:
    
    - name: "Prijs per uur verbruik"
    
    unit_of_measurement: "€/hour"
    
    state: >
    
    {{ ((states.sensor.electricity_meter_power_consumption.state | float) * (states.sensor.current_electricity_price_all_in.state | float)) }}
    
      
    
    - sensor:
    
    - name: "Verbruik Melding 1 kWh"
    
    unit_of_measurement: "kWh"
    
    state: >
    
    {{ ((states('input_number.verbruikmelding1')|float) * (states('input_number.verbruiklimietdag')|float))|round(2) }}
    
    - sensor:
    
    - name: "Verbruik Melding 2 kWh"
    
    unit_of_measurement: "kWh"
    
    state: >
    
    {{ ((states('input_number.verbruikmelding2')|float) * (states('input_number.verbruiklimietdag')|float))|round(2) }}
    
    - sensor:
    
    - name: "Verbruik Melding 3 kWh"
    
    unit_of_measurement: "kWh"
    
    state: >
    
    {{ ((states('input_number.verbruikmelding3')|float) * (states('input_number.verbruiklimietdag')|float))|round(2) }}
    
    - sensor:
    
    - name: "Verbruik Melding 4 kWh"
    
    unit_of_measurement: "kWh"
    
    state: >
    
    {{ ((states('input_number.verbruikmelding4')|float) * (states('input_number.verbruiklimietdag')|float))|round(2) }}
    
    - sensor:
    
    - name: "Verbruik Prijs indien prijsplafond"
    
    unit_of_measurement: "€"
    
    state: >
    
    {{ ((states('sensor.verbruikvandaagtotaal')|float) * 0.4|round(2)) }}
    
    - sensor:
    
    - name: "Verbruik kWh tot contract einde"
    
    unit_of_measurement: "kWh"
    
    state: >
    
    {{ ((states('sensor.verbruik_eind_stand_einde_contract_max') | float) - states('sensor.verbruik_meterstand_totaal') | float) | round(3) }}
    
    - sensor:
    
    - name: "Verbruik dagen tot contract einde "
    
    unit_of_measurement: "days"
    
    state: >
    
    {{ ((states('input_datetime.verbruik_eind_contract_datum') | as_datetime | as_local).date() - now().date()).days  }}
    
    - sensor:
    
    - name: "Verbruik dag budget op basis van verbruik"
    
    unit_of_measurement: "kWh/day"
    
    state: >
    
    {{ ((states('sensor.verbruik_kwh_tot_contract_einde') | float) / (states('sensor.verbruik_dagen_tot_contract_einde') | float) | round(0)) }}
    
    - sensor:
    
    - name: "Verbruik dag budget op basis van verbruik rounded"
    
    unit_of_measurement: "kWh/day"
    
    state: >
    
    {{ states('sensor.verbruik_dag_budget_op_basis_van_verbruik') | round(2) }}
    
    - sensor:
    
    - name: "Verbruik Sluimer inschatting 24u"
    
    unit_of_measurement: "kWh/day"
    
    state: >
    
    {{ ((states('sensor.verbruik_sluimer_24u') | float) * '24' | float) | round(2) }}
    
    sensor:
    
    - platform: statistics
    
    name: "Verbruik Sluimer 24u"
    
    entity_id: sensor.electricity_meter_power_consumption
    
    state_characteristic: value_min
    
    max_age:
    
    hours: 24
