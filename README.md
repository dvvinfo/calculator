# Mortgage loan calculator
Javascript code

```

const CREDIT_MIN = 0;
const CREDIT_MAX = 15000000;

const FIRST_CONTRIBUTION_MIN = 0;
const FIRST_CONTRIBUTION_MAX  = 15000000;

const RETURN_PERIOD_MIN = 1;
const RETURN_PERIOD_MIN_MAX  = 50;

const creditText = document.querySelector('#creditText');
const creditRange = document.querySelector('#creditRange');

const firstContributionText = document.querySelector('#firstContributionText');
const firstContributionRange = document.querySelector('#firstContributionRange');

const returnPeriodText = document.querySelector('#returnPeriodText');
const returnPeriodRange = document.querySelector('#returnPeriodRange');


const formatterNumber = new Intl.NumberFormat('ru');
const formatterCurrenty = new Intl.NumberFormat('ru', {
    style: 'currency',
    currency: 'RUB',
    minimumFractionDigits: 0
});
const formatterDate = {
    format(years) {
    years = parseInt(years);
    let count = years % 10;
    let txt = 'лет';
    if (years >= 5 && years <= 20){
        txt = 'лет';
    } else {
        if (count == 1){
            txt = 'год';
        }else {
            if (count >= 2 && count <=4){
                txt = 'год';
            }
        }
    }
    return years + ' ' + txt;
    }
};

setDoubleDependencies(
    creditText,
    creditRange,
    formatterNumber,
    formatterCurrenty,
    CREDIT_MIN,
    CREDIT_MAX);

setDoubleDependencies(
    firstContributionText,
    firstContributionRange,
    formatterNumber,
    formatterCurrenty,
    FIRST_CONTRIBUTION_MIN,
    FIRST_CONTRIBUTION_MAX );

setDoubleDependencies(
    returnPeriodText ,
    returnPeriodRange,
    formatterNumber,
    formatterDate,
    RETURN_PERIOD_MIN ,
    RETURN_PERIOD_MIN_MAX );
setReacton(
    creditText,
    creditRange,
    firstContributionText,
    firstContributionRange,
    returnPeriodText ,
    returnPeriodRange,
    mainProcess
);
mainProcess();



function setDoubleDependencies (textElement, rangeElement, formatterNumber, formatterGoal, min, max) {
    const middle = (min + max)/ 2;
    rangeElement.setAttribute('min', min);
    rangeElement.setAttribute('max', max);
    rangeElement.value = (min + max)/ 2;
    textElement.value = formatterGoal.format(middle);
    textElement.addEventListener('focus', function (event) {
        let number= '';
        for (const letter of this.value) {
            if ('0123456789'.includes(letter)) {
                number += letter

            }

        }
        number = parseInt(number);
        this.value = formatterNumber.format(number)

    });

    textElement.addEventListener('input', function (event) {
        let number= '';
        for (const letter of this.value) {
            if ('0123456789'.includes(letter)) {
                number += letter

            }

        }
        if (number < min) {
            number = min
        }
        if (number > max) {
            number = max
        }
        rangeElement.value = number;
        number = formatterNumber.format(number);
        this.value = number

    });
    textElement.addEventListener('blur', function (event) {
        let number= '';
        for (const letter of this.value) {
            if ('0123456789'.includes(letter)) {
                number += letter

            }

        }
        number = parseInt(number);
        this.value = formatterGoal.format(number)
    });
    rangeElement.addEventListener('input', function (event) {
        
        textElement.value = formatterGoal.format(parseInt(this.value))
    });

}
function setReacton(...args) {
    const handler = args.splice(-1)[0];
    for (const element of args) {
        element.addEventListener('input', function (event){
        handler.call(this, event, args.slice())
        })

    }
}
function mainProcess () {
    const credit = parseInt(creditRange.value);
    const firstContriBution = parseInt(firstContributionRange.value);
    const returnPeriod = parseInt(returnPeriodRange.value);
    //    Процентая ставка
    let percent = 10 + Math.log(returnPeriod) / Math.log(0.5);
    percent = parseInt(percent * 100 + 1) / 100;
    document.querySelector('#percentNumber').value = percent + '%';
    // Общая выплата
    let commonDebit = (credit - firstContriBution) * (1 + percent) ^  returnPeriod;
    document.querySelector('#common').textContent = formatterCurrenty.format(commonDebit);
    // Переплатв
    const subplayment = commonDebit - (credit - firstContriBution);
    document.querySelector('#subplayment').textContent = formatterCurrenty.format(subplayment);

    //    Общая суммв
    const payment = subplayment / (returnPeriod * 12);
    document.querySelector('#payment').textContent = formatterCurrenty.format(payment);
}
```


