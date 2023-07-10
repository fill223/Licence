<template>
  <div>
    <form-wizard
      color="#7367F0"
      :title="null"
      :subtitle="null"
      layout="vertical"
      finish-button-text="Закончить"
      next-button-text="Далее"
      back-button-text="Назад"
      class="wizard-vertical mb-3"
      @on-complete="formSubmitted"
    >
      <!-- account datails tab -->
      <tab-content title="Данные компании">
        <b-row>
          <b-col cols="12" class="mb-2">
            <h5 class="mb-0">Данные о компании</h5>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Полное наименование организации"
              label-for="v-username">
              <b-form-input
                id="v-username"
                placeholder="ООО 'Организация' или Индивидуальный предприниматель Фамилия Имя Отчество"
                v-model="info.FULLORG"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Организационно-правовая форма"
              label-for="v-org"
            >
              <v-select
                id="v-org"
                v-model="info.ORG"
                :options="OrgType"
                label="text"
              />
            </b-form-group>
          </b-col>
          <b-col>
            <b-form-group label="Юридический адрес" label-for="address">
              <b-form-input
                id="address"
                placeholder="Юридический адрес"
                v-model="info.PLACE"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="ОГРН" label-for="OGRN">
              <b-form-input id="OGRN" placeholder="ОГРН" v-model="info.OGRN" />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="ИНН " label-for="INN">
              <b-form-input id="INN" placeholder="ИНН" v-model="info.INN" />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="Вид запрашиваемой лицензии" label-for="v-type">
              <v-select
                id="v-type"
                v-model="info.TYPE"
                :options="TypeType"
                label="text"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="Номер телефона" label-for="Phone">
              <b-form-input
                id="Phone"
                placeholder="+7(999)999-99-99"
                v-model="info.PHONE"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="EMAIL" label-for="EMAIL">
              <b-form-input
                id="EMAIL"
                placeholder="EMAIL"
                v-model="info.EMAIL"
              />
            </b-form-group>
          </b-col>
          <b-col md="6"> 
            <b-form-group label="ФИО Подписанта" label-for="DIRFIO">
              <b-form-input
                id="DIRFIO"
                placeholder="Иванов Иван Иванович"
                v-model="info.DIRFIO"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="Должность подписанта" label-for="DIR">
              <b-form-input
                id="DIR"
                placeholder="Генеральный директор, Индивидуальный предпрниматель"
                v-model="info.DIR"
              />
            </b-form-group>
          </b-col>
          <b-col md="12">
            <b-form-group
              label="Полные реквизиты Листа записи в ЕГРЮЛ/ЕГРИП (дата выдача, кем, адрес органа)"
              label-for="LIST"
            >
              <b-form-textarea
                id="LIST"
                v-model="info.LIST"
                placeholder="Лист записи Единого государственного реестра юридических лиц, выданный 11.06.2020"
                rows="3"
                max-rows="6"
              ></b-form-textarea>
            </b-form-group>
          </b-col>   
          <b-col md="12">51347774144
            <b-form-group
              label="Полные реквизиты Свидетельства о постановке на налог. Учет (дата кем выдан, код, КПП)"
              label-for="SVID"
            >
              <b-form-textarea
                id="SVID"
                v-model="info.SVID"
                placeholder="Полные реквизиты Свидетельства о постановке на налог. Учет (дата кем выдан, код, КПП)"
                rows="3"
                max-rows="6"
              ></b-form-textarea>
            </b-form-group>
          </b-col>
        </b-row>
      </tab-content>

      <!-- personal info tab -->
      <tab-content title="госпошлина">
        <b-row>
          <b-col md="12">
            <b-form-group
              label="Реквизиты квитанции по оплате госпошлины "
              label-for="FEE"
            >
              <b-form-textarea
                id="LIST"
                v-model="info.FEE"
                placeholder="Информация об оплате"
                rows="3"
                max-rows="6"
              ></b-form-textarea>
            </b-form-group>
          </b-col>
        </b-row>
      </tab-content>
      <tab-content title="Курс">
        <b-row>
          <b-col md="6">
            <b-form-group label="Название урока" label-for="v-CLASS">
              <b-form-input
                id="v-CLASS"
                placeholder="Курс Python"
                v-model="info.CLASS"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Продолжительность программы"
              label-for="v-TIME"
            >
              <b-form-input
                id="v-TIME"
                placeholder="5 месяцев, 72 часа "
                v-model="info.TIME"
              />
            </b-form-group>
          </b-col>
          <b-col md="12">
            <b-form-group label="Мебель(нужна ли?)" label-for="OBJECTS">
              <b-form-textarea
                id="OBJECTS"
                v-model="info.OBJECTS"
                placeholder="мебель, ее количество"
                rows="3"
                max-rows="6"
              ></b-form-textarea>
            </b-form-group>
          </b-col>
          <b-col md="12">
            <b-form-group label="Тип Мебели(нужна ли?)" label-for="OBJECTSTYPE">
              <b-form-textarea
                id="OBJECTSTYPE"
                v-model="info.OBJECTSTYPE"
                placeholder="Личная/арендованная"
                rows="3"
                max-rows="6"
              ></b-form-textarea>
            </b-form-group>
          </b-col>
          <b-col md="12">
            <b-form-group label="Адрес юр. лица" label-for="LOCATION">
              <b-form-textarea
                id="LOCATION"
                v-model="info.LOCATION"
                placeholder="Адрес"
                rows="3"
                max-rows="6"
              ></b-form-textarea>
            </b-form-group>
          </b-col>
          <b-col md="12">
            <b-form-group
              label="Документ-основание возникновения права "
              label-for="DOCS"
            >
              <b-form-textarea
                id="DOCS"
                v-model="info.DOCS"
                placeholder="(реквизиты и сроки действия)"
                rows="3"
                max-rows="6"
              ></b-form-textarea>
            </b-form-group>
          </b-col>
          <b-col md="12">
            <b-form-group
              label="какое оборудование используется преподавателями в онлайн-школе "
              label-for="COMP"
            >
              <b-form-textarea
                id="COMP"
                v-model="info.COMP"
                placeholder="какое оборудование используется преподавателями в онлайн-школе "
                rows="3"
                max-rows="6"
              ></b-form-textarea>
            </b-form-group>
          </b-col>
          <b-col md="12">
            <b-form-group
              label="Являются ли они корпоративными (школа выдает) или личными."
              label-for="COMPTYPE"
            >
              <b-form-textarea
                id="COMPTYPE"
                v-model="info.COMPTYPE"
                placeholder="Корпоративные"
                rows="3"
                max-rows="6"
              ></b-form-textarea>
            </b-form-group>
          </b-col>
        </b-row>
      </tab-content>
      <tab-content title="ИС">
        <b-row>
          <b-col md="6">
            <b-form-group
              label="Электронная система обучения "
              label-for="ELEARN"
            >
              <b-form-input
                id="ELEARN"
                placeholder="Электронная система обучения "
                v-model="info.ELEARN"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Электронная система учета контингента обучающихся (это в обычных школах сейчас вводится, скорее неактуаьно)"
              label-for="ELEARN"
            >
              <b-form-input
                id="ELEARNCONT"
                placeholder="Электронная система учета контингента обучающихся (это в обычных школах сейчас вводится, скорее неактуаьно)"
                v-model="info.ELEARNCONT"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="Сеть интернет (данные)/ " label-for="NET">
              <b-form-input
                id="NET"
                placeholder="Канал широкополосного доступа к сети Интернет, тариф 'Корпорация', 3 Мбит/с"
                v-model="info.NET"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Доступные для сотрудников инструменты для создания, сохранения, доставки и использования электр.обр.ресурсов  "
              label-for="ETOOLS"
            >
              <b-form-input id="ETOOLS" placeholder="" v-model="info.ETOOLS" />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Какие электронные образовательные курсы используются "
              label-for="ECOURSE"
            >
              <b-form-input
                id="ECOURSE"
                placeholder="Электронный справочник «Система Гарант», Электронная библиотека "
                v-model="info.ETOOLS"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="Информационные системы " label-for="PROF">
              <b-form-input
                id="PROF"
                placeholder="Виды образования, уровни образования, профессии, специальности, направления подготовки"
                v-model="info.PROF"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="Информационные системы " label-for="INFSYS">
              <b-form-input
                id="INFSYS"
                placeholder="Kaspersky Endpoint Security Стандартный Программное обеспечение Windows"
                v-model="info.INFSYS"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Ссылка на интернет сервис"
              label-for="INFSYSLOC"
            >
              <b-form-input
                id="INFSYSLOC"
                placeholder="Форма владения,  пользования"
                v-model="info.INFSYSLOC"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Форма владения, пользования"
              label-for="INFSYSFORM"
            >
              <b-form-input
                id="INFSYSFORM"
                placeholder="Форма владения, пользования"
                v-model="info.INFSYSFORM"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Документ - основание возникновения права пользования "
              label-for="INFSYSDOC"
            >
              <b-form-input
                id="INFSYSDOC"
                placeholder="Реквизиты и сроки действия"
                v-model="info.INFSYSDOC"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Наличие интерактивных средств обучения"
              label-for="INTER"
            >
              <b-form-input
                id="INTER"
                placeholder="доска Смарт Борд  программа «___»."
                v-model="info.INTER"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="Ссылка на систему" label-for="INTERLOC">
              <b-form-input
                id="INTER"
                placeholder="Ссылка на сайт"
                v-model="info.INTERLOC"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Форма владения, пользования"
              label-for="INTERFORM"
            >
              <b-form-input
                id="INTERFORM"
                placeholder="Форма владения, пользования"
                v-model="info.INTERFORM"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Документ - основание возникновения права пользования"
              label-for="INTERDOC"
            >
              <b-form-input
                id="INTERDOC"
                placeholder="Реквизиты и срок действия"
                v-model="info.INTERDOC"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="Сервер" label-for="SERVER">
              <b-form-input
                id="SERVER"
                placeholder="Название, данные договора"
                v-model="info.SERVER"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="Ссылка на сервер" label-for="SERVERLOC">
              <b-form-input
                id="SERVERLOC"
                placeholder=""
                v-model="info.SERVERLOC"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="Форма владения" label-for="SERVERFORM">
              <b-form-input
                id="SERVERFORM"
                placeholder=""
                v-model="info.SERVERFORM"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Документ, основание владения"
              label-for="SERVERDOC"
            >
              <b-form-input
                id="SERVERDOC"
                placeholder=""
                v-model="info.SERVERDOC"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="Данные сети интернет" label-for="NET">
              <b-form-input id="NET" placeholder="" v-model="info.NET" />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="Ссылка/адрес NETLOC" label-for="NETLOC">
              <b-form-input id="NETLOC" placeholder="" v-model="info.NETLOC" />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group label="Форма владения" label-for="NETFORM">
              <b-form-input id="NETFORM" placeholder="" v-model="info.NETLOC" />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Документ, основание прав пользования сетью"
              label-for="NETDOC"
            >
              <b-form-input id="NETDOC" placeholder="" v-model="info.NETLOC" />
            </b-form-group>
          </b-col>
        </b-row>
      </tab-content>
      <tab-content title="Преподаватели">
        <div class="zaglav">
          Численность педагогических работников - всего
        </div>
        <b-row>
          <b-col md="6">
            <b-form-group label="Количество пед. работников" label-for="d">
              <b-form-input
                id="TEACHERNUMBER"
                placeholder="Число"
                v-model="info.TEACHERNUMBER"
              />
              <!-- <button v-on:click="plus(1)">Плюс один</button>
              111111111111111111111111111111111111111111111111111111111111111111111111111111111111 -->
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Штатные педагогические работники, за исключением  совместителей"
              label-for="shtatrabot_num"
            >
              <b-form-input
                id="shtatrabot_num"
                placeholder="Число"
                v-model="info.shtatrabot_num"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Педагогические работники, работающие на условиях  внутреннего совместительства"
              label-for="insidesovmet_num"
            >
              <b-form-input
                id="insidesovmet_num"
                placeholder="Число"
                v-model="info.insidesovmet_num"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Педагогические работники, работающие на условиях  внешнего совместительства"
              label-for="outsidesovmet_num"
            >
              <b-form-input
                id="outsidesovmet_num"
                placeholder="Число"
                v-model="info.outsidesovmet_num"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Педагогические работники, работающие на условиях  почасовой оплаты труда"
              label-for="hourpaid_num"
            >
              <b-form-input
                id="hourpaid_num"
                placeholder="Число"
                v-model="info.hourpaid_num"
              />
            </b-form-group>
          </b-col>
        </b-row>
        <div class="zaglav">
          Из общей численности педагогических работников
        </div>
        <b-row>
          <b-col md="6">
            <b-form-group
              label="Лица, имеющие ученую степень доктора наук"
              label-for="doc_degree_num"
            >
              <b-form-input
                id="doc_degree_num"
                placeholder="Число"
                v-model="info.doc_degree_num"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Лица, имеющие ученую степень кандидата наук"
              label-for="can_degree_num"
            >
              <b-form-input
                id="can_degree_num"
                placeholder="Число"
                v-model="info.can_degree_num"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Лица, имеющие высшее профессиональное образование"
              label-for="high_degree_num"
            >
              <b-form-input
                id="high_degree_num"
                placeholder="Число"
                v-model="info.high_degree_num"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Лица, имеющие среднее профессиональное образование"
              label-for="mid_degree_num"
            >
              <b-form-input
                id="mid_degree_num"
                placeholder="Число"
                v-model="info.mid_degree_num"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Лица, не имеющие профессионального образования "
              label-for="no_degree_num"
            >
              <b-form-input
                id="no_degree_num"
                placeholder="Число"
                v-model="info.no_degree_num"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group 
              label="Преподаватели" 
              label-for="regular_teacher_num">
              <b-form-input
                id="regular_teacher_num"
                placeholder="Число"
                v-model="info.regular_teacher_num"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Мастера производственного обучения"
              label-for="mas_degree_num"
            >
              <b-form-input
                id="mas_degree_num"
                placeholder="Число"
                v-model="info.mas_degree_num"
              />
            </b-form-group>
          </b-col>
        </b-row>
        <div class="zaglav">
          Численность научных работников
        </div>
        <b-row>
          <b-col md="6">
            <b-form-group
              label="Штатные научные работники, за исключением совместителей"
              label-for="shat_nauch"
            >
              <b-form-input
                id="shat_nauch"
                placeholder="Число"
                v-model="info.shat_nauch"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Научные работники, работающие на условиях  совместительства"
              label-for="sovmet_nauch_num"
            >
              <b-form-input
                id="sovmet_nauch_num"
                placeholder="Число"
                v-model="info.sovmet_nauch_num"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Научные работники, работающие на условиях почасовой оплаты труда"
              label-for="hour_nauch_num"
            >
              <b-form-input
                id="hour_nauch_num"
                placeholder="Число"
                v-model="info.hour_nauch_num"
              />
            </b-form-group>
          </b-col>
        </b-row>
        <div class="zaglav">
          Из общей численности научных работников
        </div>
        <b-row>
          <b-col md="6">
            <b-form-group
              label="Лица, имеющие ученую степень доктора наук"
              label-for="nauch_doc_degree"
            >
              <b-form-input
                id="nauch_doc_degree"
                placeholder="Число"
                v-model="info.nauch_doc_degree"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Лица, имеющие ученую степень кандидата наук"
              label-for="nauch_science_degree"
            >
              <b-form-input
                id="nauch_science_degree"
                placeholder="Число"
                v-model="info.nauch_science_degree"
              />
            </b-form-group>
          </b-col>
        </b-row>
      </tab-content>
      <tab-content title="Направления">
        <b-row>
          <b-col md="6">
            <b-form-group
              label="Название программы"
              label-for="NAME"
            >
              <b-form-input
                id="NAME"
                placeholder="Например: SSL вышка"
                v-model="info.NAME"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="ФИО Преподавателя"
              label-for="TEACHERNAME"
            >
              <b-form-input
                id="TEACHERNAME"
                placeholder="Галицкий Сергей Николаевич"
                v-model="info.TEACHERNAME"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Образование преподавателя"
              label-for="TEACHEREDU"
            >
              <b-form-input
                id="TEACHEREDU"
                placeholder="Не полноее высшее"
                v-model="info.TEACHEREDU"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Ученая степень"
              label-for="TEACHERLVL"
            >
              <b-form-input
                id="TEACHERLVL"
                placeholder="Доктор таких то наук"
                v-model="info.TEACHERLVL"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Стаж педагогики"
              label-for="TEACHEREXP"
            >
              <b-form-input
                id="TEACHEREXP"
                placeholder="7 лет 12 месяцев"
                v-model="info.TEACHEREXP"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Стаж в этой дисциплине"
              label-for="TEACHER_EXPDETAIL"
            >
              <b-form-input
                id="TEACHER_EXPDETAIL"
                placeholder="4 года"
                v-model="info.TEACHER_EXPDETAIL"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Место основной работы"
              label-for="TEACHERWORK"
            >
              <b-form-input
                id="TEACHERWORK"
                placeholder="Газпром"
                v-model="info.TEACHERWORK"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Условия работы"
              label-for="TEACHERHOW"
            >
              <b-form-input
                id="TEACHERHOW"
                placeholder="штатный работник, внутренний совместитель"
                v-model="info.TEACHERHOW"
              />
            </b-form-group>
          </b-col>
        </b-row>
      </tab-content>
      <tab-content title="Ресурсы">
        <b-row>
          <b-col md="6">
            <b-form-group
              label="Название программы"
              label-for="NAME"
            >
              <b-form-input
                id="NAME"
                placeholder="Например: SSL вышка"
                v-model="info.NAME"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Количество экземпляров учебной литературы"
              label-for="UCHEB"
            >
              <b-form-input
                id="UCHEB"
                placeholder="Число"
                v-model="info.UCHEB"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Количество экземпляров учебно-методической литературы"
              label-for="METHOD"
            >
              <b-form-input
                id="METHOD"
                placeholder="Число"
                v-model="info.METHOD"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Количество экземпляров литературы из расчета на одного обучающего-ся (воспитанника)"
              label-for="num"
            >
              <b-form-input
                id="NUM"
                placeholder="Число"
                v-model="info.NUM"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Доля фонда учебной и учебно-методической литературы, изданной не ранее последних 5 лет, от общего числа экземпляров"
              label-for="FOND"
            >
              <b-form-input
                id="FOND"
                placeholder="Вот что тут написать?"
                v-model="info.FOND"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Краткая характеристика материалов"
              label-for="SHARE"
            >
              <b-form-input
                id="SHARE"
                placeholder="Например: SSL вышка"
                v-model="info.SHARE"
              />
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Количество экземпляров"
              label-for="BOOKS"
            >
              <b-form-input
                id="BOOKS"
                placeholder="Число"
                v-model="info.BOOKS"
              />  
            </b-form-group>
          </b-col>
           <b-col md="6">
            <b-form-group
              label="Официальные издания"
              label-for="OFF"
            >
              <b-form-input
                id="OFF"
                placeholder="Число"
                v-model="info.OFF"
              />  
            </b-form-group>
          </b-col>
           <b-col md="6">
            <b-form-group
              label="Научные, отраслевые, общественно-политические, научно-популярные и иные периодические издания (журналы)"
              label-for="JOURNAL"
            >
              <b-form-input
                id="JOURNAL"
                placeholder="Число"
                v-model="info.JOURNAL"
              />  
            </b-form-group>
          </b-col>
           <b-col md="6">
            <b-form-group
              label="Справочно-библиографические издания"
              label-for="SPRAV"
            >
              <b-form-input
                id="SPRAV"
                placeholder="Число"
                v-model="info.SPRAV"
              />  
            </b-form-group>
          </b-col>
           <b-col md="6">
            <b-form-group
              label="Энциклопедии"
              label-for="ENC"
            >
              <b-form-input
                id="ENC"
                placeholder="Число"
                v-model="info.ENC"
              />  
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Отраслевые словари и справочники"
              label-for="INDUSTRY"
            >
              <b-form-input
                id="INDUSTRY"
                placeholder="Число"
                v-model="info.INDUSTRY"
              />  
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Текущие и ретроспективные отраслевые библиографические пособия (по профилю (направленности) образовательных программ)"
              label-for="RETRO"
            >
              <b-form-input
                id="RETRO"
                placeholder="Число"
                v-model="info.RETRO"
              />  
            </b-form-group>
          </b-col>
          <b-col md="6">
            <b-form-group
              label="Научная литература"
              label-for="SCIENCE"
            >
              <b-form-input
                id="SCIENCE"
                placeholder="Число"
                v-model="info.SCIENCE"
              />  
            </b-form-group>
          </b-col>
        </b-row>
      </tab-content>
    </form-wizard>
  </div>
</template>

<script>
import { FormWizard, TabContent } from "vue-form-wizard";
import vSelect from "vue-select";
import "vue-form-wizard/dist/vue-form-wizard.min.css";
import ToastificationContent from "@core/components/toastification/ToastificationContent.vue";
import Docxtemplater from "docxtemplater";
import PizZip from "pizzip";
import PizZipUtils from "pizzip/utils/index.js";
import { saveAs } from "file-saver";
function loadFile(url, callback) {
  PizZipUtils.getBinaryContent(url, callback);
}

export default {
  components: {
    FormWizard,
    TabContent,
    vSelect,
    // eslint-disable-next-line vue/no-unused-components
    ToastificationContent,
  },
  data() {
    return {
      selectedContry: "select_value",
      selectedLanguage: "nothing_selected",
      OrgType: ["ИП", "ООО"],
      TypeType: [
        "Дополнительное профессиональное образование",
        "Дополнительное образование детей и взрослых",
      ],
      info: {
        FULLORG: "",
        ORG: null,
        PLACE: "",
        OGRN: "",
        LIST: "",
        INN: "",
        SVID: "",
        TYPE: null,
        DIR: "",
        DIRFIO: "",
        DATE: this.$moment(new Date()).format("DD.MM.YY"),
        EMAIL: "",
        PHONE: "",
        FEE: "",
        CLASS: "",
        OBJECTS: "",
        OBJECTSTYPE: "",
        LOCATION: "",
        DOCS: "",
        COMP: "",
        COMPTYPE: "",
        TIME: "",
        ELEARN: "",
        ELEARNCONT: "",
        NET: "",
        ETOOLS: "",
        ECOURSE: "",
        INFSYS: "",
        INFSYSLOC: "",
        INFSYSFORM: "",
        INFSYSDOC: "",
        INTER: "",
        INTERLOC: "",
        INTERFORM: "",
        INTERDOC: "",
        SERVER: "",
        SERVERLOC: "",
        SERVERFORM: "",
        SERVERDOC: "",
        NETLOC: "",
        NETFORM: "",
        NETDOC: "",
        REQUIRE: "",
        TEACHERNUMBER: "",
        PROF: "",
        // Преподаватели
        shtatrabot_num: "",
        insidesovmet_num: "",
        outsidesovmet_num: "",
        hourpaid_num: "",
        doc_degree_num: "",
        can_degree_num: "",
        high_degree_num: "",
        mid_degree_num: "",
        no_degree_num: "",
        regular_teacher_num: "",
        mas_degree_num: "",
        TEACHERNUMBER : "",
        shat_nauch: "",
        sovmet_nauch_num: "",
        hour_nauch_num: "",
        TEACHERNUMBER: "",
        nauch_doc_degree: "",
        nauch_science_degree: "",
        // Направления
        NAME: "",
        CLASS: "",
        UCHEB: "",
        METHOD: "",
        NUM: "",
        FOND: "",
        SHARE: "",
        BOOKS: "",
        OFF: "",
        JOURNAL: "",
        SPRAV: "",
        ENC: "",
        INDUSTRY: "",
        RETRO: "",
        SCIENCE: "",
        // ОДИН ПРЕПОД
        TEACHEREDU: "",
        TEACHEREXP: "",
        TEACHER_EXPDETAIL: "",
        TEACHERWORD: "",
        TEACHERHOW: "",
        TEACHERNAME:"",
        TEACHERLVL:"",
        TEACHERWORK:"", 
      },
    };
  },
  methods: {
    plus() {
      this.TEACHERNUMBER = this.TEACHERNUMBER + 1;
    },
    loadfile1() {
      loadFile(
        "http://online.klimenko.studio/assets/1.docx",
        function(error, content) {
          if (error) {
            throw error;
          }
          var zip = new PizZip(content);
          var doc = new Docxtemplater(zip);
          doc.setData(this.info); //параметры
          try {
            // render the document (replace all occurences of {first_name} by John, {last_name} by Doe, ...)
            doc.render();
          } catch (error) {
            // The error thrown here contains additional information when logged with JSON.stringify (it contains a properties object containing all suberrors).
            function replaceErrors(key, value) {
              if (value instanceof Error) {
                return Object.getOwnPropertyNames(value).reduce(function(
                  error,
                  key
                ) {
                  error[key] = value[key];
                  return error;
                },
                {});
              }
              return value;
            }
            console.log(JSON.stringify({ error: error }, replaceErrors));

            if (error.properties && error.properties.errors instanceof Array) {
              const errorMessages = error.properties.errors
                .map(function(error) {
                  return error.properties.explanation;
                })
                .join("\n");
              console.log("errorMessages", errorMessages);
              // errorMessages is a humanly readable message looking like this :
              // 'The tag beginning with "foobar" is unopened'
            }
            throw error;
          }
          var out = doc.getZip().generate({
            type: "blob",
            mimeType:
              "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
          }); //Output the document using Data-URI
          saveAs(out, "output.docx");
        }.bind(this)
      );
    },
    loadfile2() {
      loadFile(
        "http://online.klimenko.studio/assets/2.docx",
        function(error, content) {
          if (error) {
            throw error;
          }
          var zip = new PizZip(content);
          var doc = new Docxtemplater(zip);
          doc.setData(this.info); //параметры
          try {
            // render the document (replace all occurences of {first_name} by John, {last_name} by Doe, ...)
            doc.render();
          } catch (error) {
            // The error thrown here contains additional information when logged with JSON.stringify (it contains a properties object containing all suberrors).
            function replaceErrors(key, value) {
              if (value instanceof Error) {
                return Object.getOwnPropertyNames(value).reduce(function(
                  error,
                  key
                ) {
                  error[key] = value[key];
                  return error;
                },
                {});
              }
              return value;
            }
            console.log(JSON.stringify({ error: error }, replaceErrors));

            if (error.properties && error.properties.errors instanceof Array) {
              const errorMessages = error.properties.errors
                .map(function(error) {
                  return error.properties.explanation;
                })
                .join("\n");
              console.log("errorMessages", errorMessages);
              // errorMessages is a humanly readable message looking like this :
              // 'The tag beginning with "foobar" is unopened'
            }
            throw error;
          }
          var out = doc.getZip().generate({
            type: "blob",
            mimeType:
              "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
          }); //Output the document using Data-URI
          saveAs(out, "output_second.docx");
        }.bind(this)
      );
    },
    loadfile3() {
      loadFile(
        "http://online.klimenko.studio/assets/3.docx",
        function(error, content) {
          if (error) {
            throw error;
          }
          var zip = new PizZip(content);
          var doc = new Docxtemplater(zip);
          doc.setData(this.info); //параметры
          try {
            // render the document (replace all occurences of {first_name} by John, {last_name} by Doe, ...)
            doc.render();
          } catch (error) {
            // The error thrown here contains additional information when logged with JSON.stringify (it contains a properties object containing all suberrors).
            function replaceErrors(key, value) {
              if (value instanceof Error) {
                return Object.getOwnPropertyNames(value).reduce(function(
                  error,
                  key
                ) {
                  error[key] = value[key];
                  return error;
                },
                {});
              }
              return value;
            }
            console.log(JSON.stringify({ error: error }, replaceErrors));

            if (error.properties && error.properties.errors instanceof Array) {
              const errorMessages = error.properties.errors
                .map(function(error) {
                  return error.properties.explanation;
                })
                .join("\n");
              console.log("errorMessages", errorMessages);
              // errorMessages is a humanly readable message looking like this :
              // 'The tag beginning with "foobar" is unopened'
            }
            throw error;
          }
          var out = doc.getZip().generate({
            type: "blob",
            mimeType:
              "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
          }); //Output the document using Data-URI
          saveAs(out, "output_third.docx");
        }.bind(this)
      );
    },
    loadfile4() {
      loadFile(
        "http://online.klimenko.studio/assets/4.docx",
        function(error, content) {
          if (error) {
            throw error;
          }
          var zip = new PizZip(content);
          var doc = new Docxtemplater(zip);
          doc.setData(this.info); //параметры
          try {
            // render the document (replace all occurences of {first_name} by John, {last_name} by Doe, ...)
            doc.render();
          } catch (error) {
            // The error thrown here contains additional information when logged with JSON.stringify (it contains a properties object containing all suberrors).
            function replaceErrors(key, value) {
              if (value instanceof Error) {
                return Object.getOwnPropertyNames(value).reduce(function(
                  error,
                  key
                ) {
                  error[key] = value[key];
                  return error;
                },
                {});
              }
              return value;
            }
            console.log(JSON.stringify({ error: error }, replaceErrors));

            if (error.properties && error.properties.errors instanceof Array) {
              const errorMessages = error.properties.errors
                .map(function(error) {
                  return error.properties.explanation;
                })
                .join("\n");
              console.log("errorMessages", errorMessages);
              // errorMessages is a humanly readable message looking like this :
              // 'The tag beginning with "foobar" is unopened'
            }
            throw error;
          }
          var out = doc.getZip().generate({
            type: "blob",
            mimeType:
              "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
          }); //Output the document using Data-URI
          saveAs(out, "output_fourth.docx");
        }.bind(this)
      );
    },
    loadfile5_1() {
      loadFile(
        "http://online.klimenko.studio/assets/5.1.docx",
        function(error, content) {
          if (error) {
            throw error;
          }
          var zip = new PizZip(content);
          var doc = new Docxtemplater(zip);
          doc.setData(this.info); //параметры
          try {
            // render the document (replace all occurences of {first_name} by John, {last_name} by Doe, ...)
            doc.render();
          } catch (error) {
            // The error thrown here contains additional information when logged with JSON.stringify (it contains a properties object containing all suberrors).
            function replaceErrors(key, value) {
              if (value instanceof Error) {
                return Object.getOwnPropertyNames(value).reduce(function(
                  error,
                  key
                ) {
                  error[key] = value[key];
                  return error;
                },
                {});
              }
              return value;
            }
            console.log(JSON.stringify({ error: error }, replaceErrors));

            if (error.properties && error.properties.errors instanceof Array) {
              const errorMessages = error.properties.errors
                .map(function(error) {
                  return error.properties.explanation;
                })
                .join("\n");
              console.log("errorMessages", errorMessages);
              // errorMessages is a humanly readable message looking like this :
              // 'The tag beginning with "foobar" is unopened'
            }
            throw error;
          }
          var out = doc.getZip().generate({
            type: "blob",
            mimeType:
              "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
          }); //Output the document using Data-URI
          saveAs(out, "output_fifth.1.docx");
        }.bind(this)
      );
    },
    loadfile5() {
      loadFile(
        "http://online.klimenko.studio/assets/5.docx",
        function(error, content) {
          if (error) {
            throw error;
          }
          var zip = new PizZip(content);
          var doc = new Docxtemplater(zip);
          doc.setData(this.info); //параметры
          try {
            // render the document (replace all occurences of {first_name} by John, {last_name} by Doe, ...)
            doc.render();
          } catch (error) {
            // The error thrown here contains additional information when logged with JSON.stringify (it contains a properties object containing all suberrors).
            function replaceErrors(key, value) {
              if (value instanceof Error) {
                return Object.getOwnPropertyNames(value).reduce(function(
                  error,
                  key
                ) {
                  error[key] = value[key];
                  return error;
                },
                {});
              }
              return value;
            }
            console.log(JSON.stringify({ error: error }, replaceErrors));

            if (error.properties && error.properties.errors instanceof Array) {
              const errorMessages = error.properties.errors
                .map(function(error) {
                  return error.properties.explanation;
                })
                .join("\n");
              console.log("errorMessages", errorMessages);
              // errorMessages is a humanly readable message looking like this :
              // 'The tag beginning with "foobar" is unopened'
            }
            throw error;
          }
          var out = doc.getZip().generate({
            type: "blob",
            mimeType:
              "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
          }); //Output the document using Data-URI
          saveAs(out, "output_fifth.docx");
        }.bind(this)
      );
    },
    loadfile6() {
      loadFile(
        "http://online.klimenko.studio/assets/6.docx",
        function(error, content) {
          if (error) {
            throw error;
          }
          var zip = new PizZip(content);
          var doc = new Docxtemplater(zip);
          doc.setData(this.info); //параметры
          try {
            // render the document (replace all occurences of {first_name} by John, {last_name} by Doe, ...)
            doc.render();
          } catch (error) {
            // The error thrown here contains additional information when logged with JSON.stringify (it contains a properties object containing all suberrors).
            function replaceErrors(key, value) {
              if (value instanceof Error) {
                return Object.getOwnPropertyNames(value).reduce(function(
                  error,
                  key
                ) {
                  error[key] = value[key];
                  return error;
                },
                {});
              }
              return value;
            }
            console.log(JSON.stringify({ error: error }, replaceErrors));

            if (error.properties && error.properties.errors instanceof Array) {
              const errorMessages = error.properties.errors
                .map(function(error) {
                  return error.properties.explanation;
                })
                .join("\n");
              console.log("errorMessages", errorMessages);
              // errorMessages is a humanly readable message looking like this :
              // 'The tag beginning with "foobar" is unopened'
            }
            throw error;
          }
          var out = doc.getZip().generate({
            type: "blob",
            mimeType:
              "application/vnd.openxmlformats-officedocument.wordprocessingml.document",
          }); //Output the document using Data-URI
          saveAs(out, "output_sixth.docx");
        }.bind(this)
      );
    },
    formSubmitted() {
      this.loadfile1(); 
      this.loadfile2();
      this.loadfile3();
      this.loadfile4();
      this.loadfile5();
      this.loadfile5_1();
      this.loadfile6();
    },
  },
};
</script>

<style lang="scss">
@import "@core/scss/vue/libs/vue-wizard.scss";
@import "@core/scss/vue/libs/vue-select.scss";
</style>
